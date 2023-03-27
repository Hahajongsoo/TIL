쿠버네티스 상에 프로메테우스를 설치하기 위해서 helm에 있는 kube-prometheus-stack을 사용합니다.
적용한 value 파일입니다. EKS는 controlplane을 관리형으로 제공하기 때문에 Control Plane에 있는 구성요소들인 kube-controller-manager, etcd, kube-scheduler 등에 대해서 워커 노드에서는 접근할 수 없습니다.
따라서 해당 요소들에 대해서 serviceMonitor를 생성하지 않도록 합니다. 
Istio에서 라우팅을 통해 서비스에 접근할 것이기 때문에 service 타입은 변경하지 않습니다. 기본값은 ClusterIP입니다.
기본으로 각 storage가 emptyDir로 되어있기 때문에 이를 pvc를 사용하도록 변경해줍니다. 현재 시스템에서 default storage class는 gp2로 되어있습니다. 

```yaml
kubeControllerManager:
  enabled: false

kubeEtcd:
  enabled: false

kubeScheduler:
  enabled: false

additionalPrometheusRulesMap:
  rule-name:
    groups:
      - name: test
        rules:
          - alert: CPUThrottlingHighTest
            annotations:
              description: "{{ $value | humanizePercentage }} throttling of CPU in namespace {{ $labels.namespace }} for container {{ $labels.container }} in pod {{ $labels.pod }}."
              runbook_url: https://runbooks.prometheus-operator.dev/runbooks/kubernetes/cputhrottlinghigh
              summary: Processes experience elevated CPU throttling.
            expr: |
              sum(increase(container_cpu_cfs_throttled_periods_total{container!="", namespace="default"}[5m])) by (container, pod, namespace)
                /
              sum(increase(container_cpu_cfs_periods_total{}[5m])) by (container, pod, namespace)
                > ( 25 / 100 )
            for: 1m
            labels:
              severity: critical
alertmanager:
  config:
    global:
      resolve_timeout: 5m
    inhibit_rules:
      - source_matchers:
          - "severity = critical"
        target_matchers:
          - "severity =~ warning|info"
        equal:
          - "namespace"
          - "alertname"
      - source_matchers:
          - "severity = warning"
        target_matchers:
          - "severity = info"
        equal:
          - "namespace"
          - "alertname"
      - source_matchers:
          - "alertname = InfoInhibitor"
        target_matchers:
          - "severity = info"
        equal:
          - "namespace"
    route:
      group_by: ["namespace"]
      group_wait: 30s
      group_interval: 5m
      repeat_interval: 12h
      receiver: "null"
      routes:
        - receiver: "null"
          matchers:
            - alertname =~ "InfoInhibitor|Watchdog"
        - receiver: "Slack"
          continue: true
          matchers:
            - alertname = "CPUThrottlingHighTest"
            - severity = "critical"

    receivers:
      - name: "null"
      - name: "Slack"
        slack_configs:
          - channel: "#alertmanager"
            api_url: "https://hooks.slack.com/services/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"
            icon_url: "https://avatars3.githubusercontent.com/u/3380462"
            username: "alertmanager_bot"
            send_resolved: true
            title: '[{{ .Status | toUpper }}{{ if eq .Status "firing" }}:{{ .Alerts.Firing | len }}{{ end }}] 모니터링 이벤트 알림'
            text: '{{ template "slack.myorg.text" . }}'
    templates:
      - "/etc/alertmanager/config/*.tmpl"

  ## Alertmanager template files to format alerts
  ## By default, templateFiles are placed in /etc/alertmanager/config/ and if
  ## they have a .tmpl file suffix will be loaded. See config.templates above
  ## to change, add other suffixes. If adding other suffixes, be sure to update
  ## config.templates above to include those suffixes.
  ## ref: https://prometheus.io/docs/alerting/notifications/
  ##      https://prometheus.io/docs/alerting/notification_examples/
  ##
  templateFiles:
    #
    ## An example template:
    template_1.tmpl: |-
      {{ define "slack.myorg.text" }}
      {{ range .Alerts }}
        *Alert:* {{ .Annotations.summary }} - `{{ .Labels.severity }}`
        *요약:* {{ .Annotations.description }}
        *그래프:* <{{ .GeneratorURL }}|:chart_with_upwards_trend:>
        *세부내용:*
          {{ range .Labels.SortedPairs }} - *{{ .Name }}:* `{{ .Value }}`
          {{ end }}
      {{ end }}
      {{ end }}
  alertmanagerSpec:
    externalUrl: "http://alertmanager.moamoa-news.com/"
    storage:
      volumeClaimTemplate:
        spec:
          storageClassName: gp2
          accessModes: ["ReadWriteOnce"]
          resources:
            requests:
              storage: 10Gi
grafana:
  persistence:
    type: pvc
    enabled: true
    storageClassName: gp2
    accessModes:
      - ReadWriteOnce
    size: 10Gi

prometheus:
  prometheusSpec:
    externalUrl: "http://prometheus.moamoa-new.com/"
    storageSpec:
      volumeClaimTemplate:
        spec:
          storageClassName: gp2
          accessModes: ["ReadWriteOnce"]
          resources:
            requests:
              storage: 30Gi
```
# TLS 적용하기

## AWS Certificate Manager로 인증서 생성하기
이후 AWS Certificate Manager에서 인증서를 생성하도록 합니다. 이후 모든 서브도메인에 TLS를 적용하기 위해서 와일드 카드를 사용하여 인증서를 생성합니다.

![](images/Pasted%20image%2020230322220334.png)

![](images/Pasted%20image%2020230322220347.png)

와일드 카드만 등록하면 루트도메인에 대해서는 인증서가 발급되지 않으므로 루트도메인도 추가해줘야한다. 

![](images/Pasted%20image%2020230322220430.png)

일정 시간이 지나면 도메인에 대한 상태가 검증 대기 중에서 성공으로 바뀌는 것을 확인할 수 있다.

![](images/Pasted%20image%2020230322220815.png)

## Istio VirtualService로 라우팅하기
현재 Ingress로 클러스터 내부로 들어오는 트래픽을 받고 있고 이후 Ingress 인 ALB는 Istio Ingressgateway로 트래픽을 넘겨줍니다. 하나의 ALB로 트래픽을 받고있기 때문에 내부에서는 호스트별로 다른 서비스로 라우팅할 수 있게 Istio VirtualService를 구성합니다.
```yaml
---
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: prometheus
  namespace: monitoring
spec:
  hosts:
    - prometheus.moamoa-news.com
  gateways:
    - istio-system/monitor-gateway
  http:
    - match:
        - uri:
            prefix: /
      route:
        - destination:
            host: prometheus-kube-prometheus-prometheus
            port:
              number: 9090
---
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: grafana
  namespace: monitoring
spec:
  hosts:
    - grafana.moamoa-news.com
  gateways:
    - istio-system/monitor-gateway
  http:
    - match:
        - uri:
            prefix: /
      route:
        - destination:
            host: prometheus-grafana
            port:
              number: 80
---
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: alertmanager
  namespace: monitoring
spec:
  hosts:
    - alertmanager.moamoa-news.com
  gateways:
    - istio-system/monitor-gateway
  http:
    - match:
        - uri:
            prefix: /
      route:
        - destination:
            host: prometheus-kube-prometheus-alertmanager
            port:
              number: 9093
```

이후 Route53에서 prometheus, grafana, alertmanager의 주소를 Ingress 인 ALB의 주소로 바꿔줍니다. 

![](images/Pasted%20image%2020230323152534.png)

HTTPS로 접속이 되는 것을 확인할 수 있습니다.

![](images/Pasted%20image%2020230323152632.png)

# podMonitor, serviceMonitor 추가 설치하기
prometheus operator를 사용하기 때문에 istio의 메트릭을 추가하기 위해서 podMonitor와 serviceMonitor를 추가해야합니다. 그런데 기본적으로 프로메테우스는 podMonitor와 serviceMonitor를 해당 네임스페이스에서 prometheus-operator 릴리즈 태그와 같은 태그 레이블을 가진 것을 찾습니다. 이대 네임스페이스와 레이블에 관계 없이 podMonitor와 serviceMonitor를 추가하려면 헬름 value에서 `prometheus.prometheusSpec.podMonitorSelectorNilUsesHelmValues` 와  `prometheus.prometheusSpec.serviceMonitorSelectorNilUsesHelmValues` 값을 `false`로 두면 됩니다. 
그래서 처음에는 값을 이렇게 설정하고 istio에서 제공하는 podMonitor와 serviceMonitor를 추가로 배포했으나 이렇게 하기보다는 value 파일의 `prometheus.additionalServiceMonitors` 와 `prometheus.additionalPodMonitors`에 해당 값을 추가하는 것이 더 좋다고 생각되어 해당 값을 추가한 value 파일로 설치했습니다.
```yaml
...
prometheus:
  additionalServiceMonitors:
    - name: "istio-component-monitor"
      jobLabel: "istio"
      targetLabels: [app]
      selector:
        matchExpressions:
          - { key: istio, operator: In, values: [pilot] }
      namespaceSelector:
        any: true
        matchNames: []
      endpoints:
        - port: "http-monitoring"
          interval: 30s

  additionalPodMonitors:
    - name: "envoy-stats-monitor"
      jobLabel: "envoy-stats"
      selector:
        matchExpressions:
          - { key: istio-prometheus-ignore, operator: DoesNotExist }
      namespaceSelector:
        any: true
      podMetricsEndpoints:
        - path: /stats/prometheus
          interval: 15s
          relabelings:
            - action: keep
              sourceLabels: [__meta_kubernetes_pod_container_name]
              regex: "istio-proxy"
            - action: keep
              sourceLabels:
                [__meta_kubernetes_pod_annotationpresent_prometheus_io_scrape]
            - sourceLabels:
                [
                  __address__,
                  __meta_kubernetes_pod_annotation_prometheus_io_port,
                ]
              action: replace
              regex: ([^:]+)(?::\d+)?;(\d+)
              replacement: $1:$2
              targetLabel: __address__
            - action: labeldrop
              regex: "__meta_kubernetes_pod_label_(.+)"
            - sourceLabels: [__meta_kubernetes_namespace]
              action: replace
              targetLabel: namespace
            - sourceLabels: [__meta_kubernetes_pod_name]
              action: replace
              targetLabel: pod_name
...
```

웹 UI에서 확인해보면 target이 추가되어 있는 것을 확인할 수 있습니다. 

![](images/Pasted%20image%2020230324221709.png)

또한 관련 쿼리도 제대로 작동하는 것을 확인할 수 있습니다.

![](images/Pasted%20image%2020230324221932.png)

# grafana 대시보드 추가로 import하기
Istio의 메트릭을 수집하는 servicMonitor와 podMonitor를 추가하고 나니 grafana 대시보드도 필요해졌습니다. 이 또한 헬름으로 설치시에 바로 추가할 수 있도록 하고자 했습니다.ㅔ
kube-prometheus-stack은 grafana/grafana 헬름 차트를 dependency로 갖고 있기 때문에 해당 차트의 value 파일을 참조해도 됩니다. 해당 value 파일을 참고하면 dashboard provider와 dashboards 필드를 사용해서 grafana dashboard 웹사이트에 있는 대시보드들을 import 할 수 있습니다.
```yaml
grafana:
  dashboardProviders:
    dashboardproviders.yaml:
      apiVersion: 1
      providers:
        - name: "default"
          orgId: 1
          folder: ""
          type: file
          disableDeletion: false
          editable: true
          options:
            path: /var/lib/grafana/dashboards/default
  dashboards:
    default:
      Istio-Control-Plane-Dashboard:
        gnetId: 7645
        revision: 158
        datasource: Prometheus
      Istio-Service-Dashboard:
        gnetId: 7636
        revision: 158
        datasource: Prometheus
      Istio-Workload-Dashboard:
        gnetId: 7630
        revision: 158
        datasource: Prometheus
      Istio-Mesh-Dashboard:
        gnetId: 7639
        revision: 158
        datasource: Prometheus
```

웹 UI에서 확인하면 곧바로 추가돼있는 것을 확인할 수 있습니다.

![](images/Pasted%20image%2020230324221456.png)

또한 파드의 컨테이너에서 로그로도 확인해볼 수 있습니다.

![](images/Pasted%20image%2020230324221613.png)