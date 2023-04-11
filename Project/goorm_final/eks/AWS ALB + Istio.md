이번 프로젝트에서 Istio와 AWS ALB를 같이 사용하기 위해서 적용한 방법에 대해서 설명하도록 하겠습니다.
현재 프로젝트에서 사용하는 환경은 다음과 같습니다.
- AWS EKS
	- kubernetes 1.25.6
- EKS 클러스터 내 AWS Load Balancer Controller 설치
- EKS 클러스터 내 cert-manager 설치
- Amazon Route 53에 도메인 등록
- Istioctl 설치

# 문제
## 배경
AWS Load Balancer Controller 를 사용하고 있기 때문에 type이 LoadBalancer인 k8s Service와 k8s Ingress를 생성할때 annotation을 지정하여 AWS ALB와 NLB를 모두 사용할 수 있습니다. 그런데 문제는 바로 Istio를 도입하면서 발생하게 됩니다. Istio를 설치하면서 Insgress gateway를 사용하게 되면 관련한 Pod와 Service가 배포됩니다. 즉 EKS 상에서 k8s Ingress를 사용하지 않기 때문에 AWS ALB를 사용할 수 없다는 문제가 발생하게 됩니다. 애플리케이션을 배포할 때, AWS ALB를 이용하여 사용할 수 있는 다른 기능들 (예를 들어 AWS WAF)을 사용하지 못하기 때문에 AWS ALB도 같이 사용하는 방법을 찾고자 했습니다. 

## 해결 방안
Istio Ingress Gateway를 type이 NodePort인 k8s service로 생성하고 이를 k8s Ingress를 통해서 생성된 AWS ALB와 연결해주면 문제가 해결됩니다. 그리고 모든 통신을 https로 하기 위해서 AWS ALB에 ACM을 사용하여 생성한 인증서를 적용하고 AWS ALB와 Istio Ingress Gateway 사이에도 cert-manager 를 사용하여 생성한 인증서를 적용합니다. 이를 도식화하면 다음과 같습니다.

![](images/eks-istio.drawio.png)

# 적용
## 1. AWS Certificate Manager를 이용한 인증서 발급
먼저 Route53에 도메인이 등록되어 있는 것을 전제로 합니다. Route 53을 이용하여 인증서를 발급 받게되면 좋은 점 중 하나는 바로 와일드카드를 이용할 수 있다는 것입니다. 서브도메인을 만들 때 마다 인증서를 발급받을 필요없이 와일드카드 패턴으로 인증서를 발급받으면 모든 서브도메인에 대해 인증서가 적용됩니다. 

1. AWS Certificate Manger로 이동하여 인증서를 요청합니다.

![](images/Pasted%20image%2020230322220334.png)

![](images/Pasted%20image%2020230322220347.png)

2. 인증서 요청시 도메인 이름에 와일드카드를 사용하여 모든 서브도메인들에 대해서 인증서가 적용되도록 합니다.

![](images/Pasted%20image%2020230322220430.png)

3. 일정 시간이 지나면 도메인에 대한 상대가 성공으로 바뀌는 것을 확인할 수 있습니다. 그리고 이후 ALB를 생성할 때 여기 나와있는 ARN을 이용하여 인증서를 적용함을 알아둬야합니다.

![](images/Pasted%20image%2020230322220815.png)

## 2. cert-manager를 이용하여 istio gateway 용 인증서 발급받기
1. cert-manager에서 사용된 manifast 는 다음과 같습니다. cert-manager 적용 내용은 [링크](cert-manager%20사용.md)에서 확인할 수 있습니다.
```yaml
---
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt
  namespace: cert-manager
spec:
  acme:
    privateKeySecretRef:
      name: letsencrypt-staging
    server: https://acme-v02.api.letsencrypt.org/directory
    email: gkwhdtn95051@gmail.com
    solvers:
      - dns01:
          route53:
            region: ap-northeast-2
            hostedZoneID: Z08092432XQCIVY7ERHU7
            accessKeyID: AKIAZ72NSUCESVG47F6A
            secretAccessKeySecretRef:
              name: route53-secret
              key: secret-access-key
        selector:
          dnsZones:
            - "moamoa-news.com"
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: moamoa-cert
  namespace: istio-system
spec:
  secretName: moamoa-cert
  dnsNames:
    - "*.moamoa-news.com"
    - "moamoa-news.com"
  issuerRef:
    name: letsencrypt
    kind: ClusterIssuer
```

2. `kubectl apply -f <filename.yaml>` 을 이용하여 해당 리소스들을 생성합니다.

3. `kubectl describe certificate -n cert-manager moamoa-cert` 명령으로 인증서가 생성되는 과정을 확인할 수 있습니다. 그리고 해당 네임스페이스에 인증서와 키가 sercret으로 생성됩니다.

```
Events:
  Type    Reason     Age    From                                       Message
  ----    ------     ----   ----                                       -------
  Normal  Issuing    3m15s  cert-manager-certificates-trigger          Issuing certificate as Secret does not exist
  Normal  Generated  3m15s  cert-manager-certificates-key-manager      Stored new private key in temporary Secret resource "moamoa-cert-bnpd9"
  Normal  Requested  3m15s  cert-manager-certificates-request-manager  Created new CertificateRequest resource "moamoa-cert-wkqmd"
  Normal  Issuing    2s     cert-manager-certificates-issuing          The certificate has been successfully issued
```

```sh
❯ kubectl describe secrets -n istio-system moamoa-cert
Name:         moamoa-cert
Namespace:    istio-system
Labels:       <none>
Annotations:  reflector.v1.k8s.emberstack.com/auto-reflects: True
              reflector.v1.k8s.emberstack.com/reflected-at: "2023-03-23T04:36:27.4193224+00:00"
              reflector.v1.k8s.emberstack.com/reflected-version: 785051
              reflector.v1.k8s.emberstack.com/reflects: cert-manager/moamoa-cert

Type:  kubernetes.io/tls

Data
====
tls.crt:  5619 bytes
tls.key:  1679 bytes
```

## 3. Istio 설치

1. `istioctl` 명령을 이용하여 istio를 설치합니다. 이때 사용한 profile 파일은 다음과 같습니다. 파일 내용은 
```sh
istioctl profile dump default > istio-default.yaml
```
을 이용하여 받은 profile 정보에 ingressgateway의 type을 NodePort로 변경하고, Istio Proxy의 리소스 requests와 limits를 변경한 내용입니다.
```sh
❯ cat istio-default.yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
spec:
  components:
    base:
      enabled: true
    cni:
      enabled: false
    egressGateways:
      - enabled: false
        name: istio-egressgateway
    ingressGateways:
      - enabled: true
        name: istio-ingressgateway
    istiodRemote:
      enabled: false
    pilot:
      enabled: true
  hub: docker.io/istio
  meshConfig:
    defaultConfig:
      proxyMetadata: {}
    enablePrometheusMerge: true
  profile: default
  tag: 1.17.1
  values:
    base:
      enableCRDTemplates: false
      validationURL: ""
    defaultRevision: ""
    gateways:
      istio-ingressgateway:
        autoscaleEnabled: true
        env: {}
        name: istio-ingressgateway
        secretVolumes:
          - mountPath: /etc/istio/ingressgateway-certs
            name: ingressgateway-certs
            secretName: istio-ingressgateway-certs
          - mountPath: /etc/istio/ingressgateway-ca-certs
            name: ingressgateway-ca-certs
            secretName: istio-ingressgateway-ca-certs
        type: NodePort
    global:
      configValidation: true
      defaultNodeSelector: {}
      defaultPodDisruptionBudget:
        enabled: true
      defaultResources:
        requests:
          cpu: 10m
      imagePullPolicy: ""
      imagePullSecrets: []
      istioNamespace: istio-system
      istiod:
        enableAnalysis: false
      jwtPolicy: third-party-jwt
      logAsJson: false
      logging:
        level: default:info
      meshNetworks: {}
      mountMtlsCerts: false
      multiCluster:
        clusterName: ""
        enabled: false
      network: ""
      omitSidecarInjectorConfigMap: false
      oneNamespace: false
      operatorManageWebhooks: false
      pilotCertProvider: istiod
      priorityClassName: ""
      proxy:
        autoInject: enabled
        clusterDomain: cluster.local
        componentLogLevel: misc:error
        enableCoreDump: false
        excludeIPRanges: ""
        excludeInboundPorts: ""
        excludeOutboundPorts: ""
        image: proxyv2
        includeIPRanges: "*"
        logLevel: warning
        privileged: true
        readinessFailureThreshold: 30
        readinessInitialDelaySeconds: 1
        readinessPeriodSeconds: 2
        resources:
          limits:
            cpu: 300m
            memory: 256Mi
          requests:
            cpu: 100m
            memory: 128Mi
        statusPort: 15020
        tracer: zipkin
      proxy_init:
        image: proxyv2
        resources:
          limits:
            cpu: 2000m
            memory: 1024Mi
          requests:
            cpu: 10m
            memory: 10Mi
      sds:
        token:
          aud: istio-ca
      sts:
        servicePort: 0
      tracer:
        datadog: {}
        lightstep: {}
        stackdriver: {}
        zipkin: {}
      useMCP: false
    istiodRemote:
      injectionURL: ""
    pilot:
      autoscaleEnabled: true
      autoscaleMax: 5
      autoscaleMin: 1
      configMap: true
      cpu:
        targetAverageUtilization: 80
      enableProtocolSniffingForInbound: true
      enableProtocolSniffingForOutbound: true
      env: {}
      image: pilot
      keepaliveMaxServerConnectionAge: 30m
      nodeSelector: {}
      podLabels: {}
      replicaCount: 1
      traceSampling: 1
    telemetry:
      enabled: true
      v2:
        enabled: true
        metadataExchange:
          wasmEnabled: false
        prometheus:
          enabled: true
          wasmEnabled: false
        stackdriver:
          configOverride: {}
          enabled: false
          logging: false
          monitoring: false
          topology: false

```

```sh
istioctl install -f istio-default.yaml
```

## 4. Ingress 배포
1. kubenetes ingress를 다음의 내용으로 배포합니다.  이때 `.metadata.annotations` 에서`alb.ingress.kubernetes.io/certificate-arn` 값은 Amazon Certificate Manager의 인증서에서 확인한 arn 값을 넣어줘야 합니다.
	- annotaions의 값으로 AWS ALB를 어떻게 설정할 수 있는지는 [링크](https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.4/guide/ingress/annotations/) 에서 확인 가능합니다.
	- healthcheck-path는 istio-proxy와 연관되어 있습니다.
	- actions.\<name> 의 name을 `rules.[*].http.paths.[*].backend.service.name`에 넣어줘야 redirect할 수있습니다.
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: istio-ingressgateway-alb
  namespace: istio-system
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: instance
    alb.ingress.kubernetes.io/backend-protocol: HTTPS
    alb.ingress.kubernetes.io/healthcheck-path: /healthz/ready
    alb.ingress.kubernetes.io/healthcheck-port: status-port
    alb.ingress.kubernetes.io/healthcheck-protocol: HTTP
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS":443}]'
    alb.ingress.kubernetes.io/actions.ssl-redirect: |
      {
        "Type": "redirect",
        "RedirectConfig": {
        "Protocol": "HTTPS",
        "Port": "443",
        "StatusCode": "HTTP_301"
        }
      }
    alb.ingress.kubernetes.io/certificate-arn: |
      arn:aws:acm:<region>:<account-ID>:certificate/*****************
spec:
  rules:
    - http:
        paths:
          - backend:
              service:
                name: ssl-redirect
                port:
                  name: use-annotation
            path: /
            pathType: Prefix
          - backend:
              service:
                name: istio-ingressgateway
                port:
                  number: 443
            path: /
            pathType: Prefix

```

![](images/Pasted%20image%2020230322220815.png)

## 5. Istio gateway 리소스 배포
다음의 내용으로 Istio gateway 리소스를 배포합니다. `.spec.servers[*].tls.credentialName` 값은 cert-manager에 의해 생성된 secret의 이름을 넣어줍니다.

```yaml
apiVersion: networking.istio.io/v1beta1
kind: Gateway
metadata:
  name: monitor-gateway
  namespace: istio-system
spec:
  selector:
    istio: ingressgateway
  servers:
    - port:
        number: 80
        name: http
        protocol: HTTP
      hosts:
        - "*"
      tls:
        httpsRedirect: true
    - port:
        number: 443
        name: https
        protocol: HTTPS
      hosts:
        - "*"
      tls:
        credentialName: moamoa-cert
        mode: SIMPLE
```

## 6. Routing할 서비스 지정 및 Route53에 레코드 등록
1. istio virtualService를 생성합니다. 현재 저희 프로젝트에서는 default 네임스페이스에 gatewayserver-service 라는 이름의 서비스가 존재하고 해당 서버에서 여러 요청들을 처리합니다. 다음 내용의 파일을 적용합니다.
```yaml
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: gateway-server
spec:
  hosts:
    - moamoa-news.com
    - www.moamoa-news.com
  gateways:
    - istio-system/monitor-gateway
  http:
    - match:
        - uri:
            prefix: /
      route:
        - destination:
            host: gatewayserver-service
            port:
              number: 8072
```

2. 위 파일에서 호스트로 지정되어 있는 이름으로 Route53에 레코드를 등록합니다. 이때 별칭으로 AWS ALB에 연결해주면 위에서 생성된 AWS ALB로 요청이 가게 됩니다.

![](images/Pasted%20image%2020230411160835.png)

# 확인
1. postman을 이용하여 요청을 보내보면 제대로 작동하는 것을 확인할 수있습니다.

![](images/Pasted%20image%2020230411160944.png)

2. kiali에서 확인되는 요청에 대한 구조는 다음과 같습니다.

![](images/Pasted%20image%2020230411161225.png)

참고
- https://aws.amazon.com/ko/blogs/containers/secure-end-to-end-traffic-on-amazon-eks-using-tls-certificate-in-acm-alb-and-istio/
- https://itnext.io/deploying-an-istio-gateway-with-tls-in-eks-using-the-aws-load-balancer-controller-448812e081e5