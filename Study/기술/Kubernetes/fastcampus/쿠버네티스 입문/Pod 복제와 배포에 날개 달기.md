# ReplicaSet 소개

## ReplicaSet 개념과 특징
ReplicaSet은 Pod 복제본을 생성하고 관리한다 
- 더 이상 N개의 Pod을 생성하기 위해 생성 명령을 N번 실행할 필요가 없다.
- ReplicaSet 오브젝트를 정의하고 원하는 Pod의 개수를 replicas 속성으로 선언한다.
- 클러스터 관리자 대신 Pod 수가 부족하거나 넘치지 않게 Pod 수를 조정
### ReplicaSet의 필요성
Pod에 문제가 생겼을 때, 서비스 다운 타임이 발생할 수 밖에 없다.
- Pod은 즉시 종료되고 클라이언트 요청을 처리할 수 없다. (No self-healing)
- 클러스터 관리자가 24/7 동안 Pod 상태를 감시하고 정상 복구해야 한다. 따라서 지속적인 수고와 자원이 들어갈 수 밖에 없다.
- N개의 Pod을 실행하고 상태 이상에 대비할 필요가 있다.
- 쿠버네티스를 이용해서 이러한 장애 상황을 자동으로 복구해주는 환경을 만들 수 있다.

## ReplicaSet과 내결함성

### 결함에 내성을 가진 서비스 환경 만들기
소프트웨어가 내결함성을 가진다 - fault tolerance
- 소프트웨어나 하드웨어 실패가 발생하더라도 소프트웨어가 정상적인 기능을 수행할 수 있어야 한다.
- 소프트웨어가 내결함성이 없으면 고객 요구사항을 만족시킬 수 없다.
- 사람의 개입없이 내결함성을 가진 소프트웨어를 구성할 수는 없을까?

### Replicaset의 역할
Pod/노드 상태에 따라 Pod의 수를 조정할 수 있도록 ReplicaSet에게 역할을 위임한다.
- ReplicaSet을 이용해 Pot 복제 및 복구 작업을 자동화
- 클러스터 관리자는 ReplicaSet을 만들어 필요한 Pod의 개수를 쿠버네티스에게 선언한다.
- 쿠버네티스가 ReplicaSet 요청서에 선언된 replicase를 읽고 그 수만큼 Pod 실행을 보장
	- Replicas : 레플리카 셋의 개수
	- Pod Selector : 레플리카 셋이 관리해야하는 Pod Selector 즉 label의 값을 넣어줘야한다.
	- Pod Template : 따로 Pod yaml을 작성할 필요없이 template을 넘겨주면 된다.

## ReplicaSet 오브젝트 표현방법
```yaml
apiVersion: apps/v1 # kubernetes API 버전
kind: ReplicaSet    # 오브젝트 타입
metadata:
  name: my-app
  labels:
    app: my-app
spec:               # 사용자가 원하는 Pod의 바람직한 상태
  selector:         # ReplicaSet이 관리해야하는 Pod을 선택하기 위한 label query
    matchLabels:
      app: blue-app # Pod label query 작성
  replicas: 3       # 실행하고자 하는 Pod 복제본 개수 선언
  template:         # Pod 실행 정보 - Pod Template과 동일
    metadata:
      labels:
        app: blue-app # ReplicaSet selector에 정의한 label을 포함해야한다.
    spec:
      containers:
      - name: blue-app
        image: blue-app:1.0
```

- ReplicaSet을 이용해서 Pod 복제본을 생성하고 관리한다.
	- 여러 노드에 걸쳐 배포된 Pod Up/Down 상태를 관리하고 replicas 수만큼 실행을 보장한다.
- ReplicaSet의 `spec.selector.matchLabels` 는 Pod teplate 부분의 `spec.template.metadata.labels` 와 같아야 한다.
- `spec.replicas` 를 선언하지 않으면 기본값은 1이다.

# ReplicaSet 실습

## ReplicaSet 생성과 배포
- ReplicaSet 생성 결과 - Pod 목록 조회
	- `kubectl get rs blue-replicaset`
	- `kubeclt get po`
- ReplicaSet의 Pod 생성 과정 확인
	- `kubectl describe rs blue-replicaset`
- ReplicaSet의 Pod 생성 과정 확인(Pod 생성 이후)
	- `kubectl get events --sort-by=.metadata.creationTimestpamp`
- Pod/sky 요청 및 응답 확인
	- `kubectl port-forward rs/blue-replicaset 8080:8080`
	- blue-replicaset 이라는 레플리카셋에 의해 생성된 파드로 트래픽 전달
	- 첫번째 생성된 파드로만 요청이 전달된다. 로드밸런싱이 일어나지 않는다.

## 기존에 생성한 Pod을 ReplicaSet으로 관리하기
- app=blue-app 레이블을 가진 단독 파드 생성
	- `kubectl apply -f blue-app.yaml`
	- `kubectl get pod`
- app=blue-app 레이블인 Pod을 관리하는 ReplicaSet 생성
	- `kubectl apply -f replicaset.yaml`
- ReplicaSet의 Pod 생성 과정 확인
	- `kubectl describe rs blue-replicaset`
	- replicaset이 pod을 두 개만 만들었다는 것을 확인할 수 있다.
	- ReplicaSdet은 자신이 관리하는 Pod의 수를 replicas를 넘지 않게 관리하는 것이다.
	- 이미 생성된 Pod의 레이블이 ReplicaSet의 Pod Selector와 같다면 관리 범주에 들어오므로 Pod Selector를 설계할 때 주의해야 한다.

## ReplicaSet Pod 종료 
- 알 수 있는 사실: ReplicaSet을 통한 Pod 생성/복구 자동화 가능
- pod 삭제시 ReplicaSet의 행동
	- Pod의 개수가 선언된 replicas와 일치하지 않으면 새로운 pod을 생성하여 replicas를 맞춘다.
- 노드 실패시 ReplicaSet의 행동
	- 노드 실패시 Up상태의 Pod의 개수가 변경되었음을 인지하고 새로운 Pod을 건강한 노드에 생성하여 replicas를 맞춘다.
- ReplicaSet 삭제 명령어 비교
	- `kubectl delete rs blue-replicaset` : replica set이 관리하는 모든 pod이 삭제된다.
	- `kubectl delete rs blue-replicaset --cascade=orphan` : pod는 남기고 replica set 삭제
- Gracefully 하게 rs 제거
	- `kubectl scale rs/blue-replicaset --replicase=0`
	- `kubectl delete rs/blue-replicset`

## ReplicaSet Pod Template 변경
- Pod template을 변경해도 기존 Pod에는 영향을 주지 않는다.
	- replecaSet에 선언한 replicas 값이 변경 되었을 경우에만 Pod을 새로 생성하거나 제거한다.

## ReplicaSet 레플리카 수 변경

## 롤백

### Pod Template 이미지 변경을 통한 롤백
- 실행 중인 Pod 장애시 ReplicaSet을 새로 생성하지 않고 이전 버전의 Pod을 배포할 수 있다. ReplicaSet이 관리하는 Pod에 결함이 생겼을 때 unmanged Pod으로 변경하고 안전하게 디버깅할 수 있다.
- 1.0 버전으로 롤백을 위해 ReplicaSet의 Pod Template 변경
	- `kubectl set image rs/<replicaset> <container>=<image>`
- 실행 중인 2.0 버전의 모든 파드 Label 변경
	- `kubectl label pod myapp-replicaset-~~ app=to-be-fixed --overwrite`
- 기존 파드는 replicaset의 관리 범주에서 벗어나지만 종료되지는 않는다.

### ReplicaSet의 replicas 조정을 통한 롤백
