# Seldon Core

- 셀든코어는 ML모델을 준비된 REST/gRPC 마이크로 서비스 제품으로 변환시켜준다.
- 셀든코어의 주요 구성은 다음과 같다.
    - eusable and non-reusable [model servers](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/overview.html#e2e-serving-with-model-servers)
    - [Language Wrappers](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/overview.html#language-wrappers) to containerise models
    - [SeldonDeployment](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/overview.html#seldondeployment-crd) CRD and [Seldon Core Operator](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/overview.html#seldon-core-operator)
    - [Service Orchestrator](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/overview.html#service-orchestrator) for advanced inference graphs
- 또한 서드파티 시스템과의 연결도 가능하다.
    - Kubernetes Ingress integration with [Ambassador](https://www.getambassador.io/) and [Istio](https://istio.io/)
    - [Metrics](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/overview.html#metrics-with-prometheus) with [Prometheus](https://prometheus.io/)
    - [Tracing](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/overview.html#distributed-tracing-with-jaeger) with [Jaeger](https://www.jaegertracing.io/)
    - [Endpoint Documentation](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/overview.html#endpoint-documentation) with [OpenApi](https://swagger.io/docs/specification/about/)
    

### Seldon Deployment CRD

seldon deployment CRE 는 셀든 코어의 진정한 강점이다. 이는 추론 모델을 쿠버네티스 클러스터에 더욱 쉽게 배포할 수 있도록 도와주고 실제 제품의 트래픽을 처리하는 도움을 준다.

기본적으로 Custom Resource는 쿠버네티스 API의 확장이다. 이는 함께 동작하는 쿠버네티스 오브젝트의 커스텀 조합을 누군가가 생성할 수 있게 한다. 셀든코어에서는 CRD를 추론 그래프를 정의하기 위해서 manifest yaml  파일과 함께 사용한다.

manifest 파일은 매우 강력하지만 간단하다. 배포에서 원하는 동작을 하는 모델을 쉽게 정의할 수 있게 하고 어떻게 그 모델들이 추론 그래프에서 연결되어있는지 쉽게 정의할 수 있다.

CRD를 클러스터에 생성된 실제 deployment 와 service 의 축약이라고 생각할 수 있다. manfest가 클러스터에 apply 되면, 셀든코어 operator는 추론 요청을 서빙하기 위해 필요한 쿠버네티스 오브젝트들을 생성한다.

Read more about [Seldon Deployment CRD on its dedicated documentation page](https://docs.seldon.io/projects/seldon-core/en/latest/reference/seldon-deployment.html).

### Seldon Core Operator

셀든코어 오퍼레이터는 쿠버네티스 클러스터에 있는 seldon deployment를 제어한다. 이는 클러스터에 적용되어 있는 seldon deployment의 CRD 정의를 읽고, 생성된 Pod와 Service 같은 생성된 모든 구성 요소들을 관리한다.

셀든코어 오퍼레이터는 일반적인 쿠버네티스 오퍼레이터 패턴 처럼 동작한다.

- 클러스터의 현재 상태를 관찰(Observe)한다.
- desired state와의 차이(Diff)를 본다.
- 필요하다면 desired state를 적용할 행동(Act)을 한다.

셀든코어 오퍼레이터의 역할

- 쿠버네티스 리소스를 생성한다. Seldon Deployment CRD로 부터 Deployment 들을 생성하고 지속적으로 그것들의 state를 조정한다.
- Orchestrator 와 InitContainer를 Pods에 추가한다. InitContainer는 model artifact들을 미리 다운로드한다.
- rolling update를 관리한다. 컨테이너나 deployment의 이름이 바뀌지 않는한 계속 관리한다.
- replica들의 스케일링을 관리한다. predictor들도 마찬가지
- ingress를 구성한다.
- Pod안에 있는 각각의 컨테이너에 포트를 할당하고 Orchestrator가 그것들을 알도록 한다.

### Service Orchestrator

서비스 오케스트레이터는 intra-graph 트래픽 관리를 담당한다. 서비스 오케스트레이터는 CRD에서 inference graph 구조를 읽고 추론 요청을 받았을 때, 그래프의 각각의 노드에 적절한 순서로 통과되도록 한다.

Seldon 에서 router, combiner, transformer 같은 복잡한 그래프 구성요소들을 사용할 수 있다.