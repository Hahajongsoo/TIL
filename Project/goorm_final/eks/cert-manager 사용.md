기존 방식은 ALB에서 istio ingress gateway로 트래픽이 갈 때 HTTP 통신으로 진행했었지만 이 통신도 HTTPS로 바꾸고자했습니다. cert manager에서는 DNS01 solver를 제공하는데 이를 이용하면 서브도메인들에 대한 인증서를 와일드카드로 한 번에 처리할 수 있기 때문에 이를 사용했습니다. 또한 다양한 provider들을 제공하고 그 중 Route53도 제공하기 때문에 기존에 사용하던 Route53을 그대로 사용합니다.
issuer로는 클러스터 전체에서 관리가 가능한 ClusterIssuer를 사용하도록 합니다. 웹서버와 모니터링 등 다양한 구성요소들이 다른 네임스페이스를 사용하고 있기 때문입니다. 그리고 서버로는 무료인 letsencrypt를 사용하도록 하겠습니다. 

# cert-manager 이용한 cert 생성
### IAM User 생성
먼저 AWS Route53을 사용하기 위해서 IAM Role과 User를 생성해야 합니다. DNS01 challenge를 수행하기 위해서는 Route53에 대한 권한이 필요합니다. 먼저 관련된 정책을 만듭니다. 
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "route53:GetChange",
      "Resource": "arn:aws:route53:::change/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "route53:ChangeResourceRecordSets",
        "route53:ListResourceRecordSets"
      ],
      "Resource": "arn:aws:route53:::hostedzone/*"
    },
    {
      "Effect": "Allow",
      "Action": "route53:ListHostedZonesByName",
      "Resource": "*"
    }
  ]
}
```
이후에는 해당 정책을 사용하는 user를 생성하도록 합니다. 이후 액세스 키를 발급받아 ClutserIssuer에서 해당 계정에 접근할 수 있게 해야합니다.

### secret 생성
aws iam user의 access-key에 대한 secret을 생성합니다.
```
kubectl create secret generic route53-secret --namespace=cert-manager --from-literal=secret-access-key=<Your ACCESS Key>
```

### ClusterIssuer 및 Certificate 생성
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

ClusterIssuer의 `hostedZoneID`에는 해당 호스팅 영역의 ID를 적어주도록 합니다.

![](images/Pasted%20image%2020230323120626.png)

그리고 해당 인증서는 `Certificate` 리소스가 생성되는 namespace의 secret으로 생성됩니다.
kubectl describe로 certificate의 event를 확인하면 인증서와 키가 생성되는 과정을 확인할 수 있습니다.
```
Events:
  Type    Reason     Age    From                                       Message
  ----    ------     ----   ----                                       -------
  Normal  Issuing    3m15s  cert-manager-certificates-trigger          Issuing certificate as Secret does not exist
  Normal  Generated  3m15s  cert-manager-certificates-key-manager      Stored new private key in temporary Secret resource "moamoa-cert-bnpd9"
  Normal  Requested  3m15s  cert-manager-certificates-request-manager  Created new CertificateRequest resource "moamoa-cert-wkqmd"
  Normal  Issuing    2s     cert-manager-certificates-issuing          The certificate has been successfully issued
```

# ALB 설정 변경
ALB를 생성할 때 사용했던 ingress의 내용을 바꿔서 HTTPS로 통신하도록 해줍니다.

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
      arn:aws:acm:ap-northeast-2:686820597897:certificate/8ab7dcfd-dbf2-42d4-ab46-e88f917e4cde
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

기존 istio gateway는 http에 대한 설정으로 되어있고 ALB는 HTTPS로 트래픽을 넘겨주기 때문에 내부 서비스에 접근이 불가능한 것을 확인할 수 있습니다.

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
```

![](images/Pasted%20image%2020230323151434.png)

istio gateway에 tls 설정을 추가해주면 이후 내부 서비스에 접근이 가능한 것을 확인할 수 있습니다. 

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: httpbin-gateway
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

![](images/Pasted%20image%2020230323151215.png)