# Pod 소개

## Pod 개념과 특징
- Pod은 여러 컨테이너를 감싸고 있는 콩 껍질과 같다.
	- 노드에서 컨테이너를 실행하기 위한 가장 기본적인 배포 단위
	- 여러 노드에 1개 이상의 pod을 분산 배포/실행 가능(Pod Relicas)
- Pod의 특징
	![](images/Pasted%20image%2020221104155350.png)
	- 쿠버네티스는 Pod을 생성할 때 노드에서 유일한 IP를 할당 (서버에서 여러개의 프로세스를 실행하는 것 처럼 하나의 서버를 운영하는 개념, 서버 분리 효과)
	- Pod 내부 컨테이너 간에 localhost로 통신 가능, 포트 충돌 주의!
	- Pod 안에서 네트워크와 볼륨 등 자원 공유
- PodIP의 특징
	- PodIP는 클러스터 안에서만 접근할 수 있다. 외부에서 PodIP로 요청을 실행하게 되면 찾을 수 없는 IP이므로 트래픽을 전송할 수 없게 된다.
	- 클러스터 외부 트래픽을 받기 위해서는 Service 혹은 Ingress 오브젝트가 필요하다.

## Pod 단위 배포와 스케일 아웃
쿠버네티스에서는 굉장히 쉬운 방법으로 Pod 복제본 즉, 여러개의 어플리케이션을 여러 노드에 분산시켜서 실행시킬 수 있는 방법을 제공한다.

- 단 하나의 명령어로 원하는 수 만큼 Pod 생성
```shell
krc@krcui-iMac-8 chapter01 % kubectl scale -f 06_deployment.yaml --replicas=3
deployment.apps/nginx-deployment scaled

krc@krcui-iMac-8 chapter01 % kubectl get po
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-65b4746994-bnwxl   1/1     Running   0          39m
nginx-deployment-65b4746994-ljfj2   1/1     Running   0          37m
nginx-deployment-65b4746994-wz88r   1/1     Running   0          39m
```
굉장히 간편한 방법으로 배포하는 과정을 단순화하고 자동화한다는 것을 알 수 있다.

### Pod과 컨테이너 설계시 고려할 점
- Pod : Container = 1 : 1 or 1 : N 결정
	1. 컨테이너들의 라이프사이클이 같은가?
		- Pod 라이프사이클 = 컨테이너들의 라이프사이클
		- 컨테이너 A가 종료되었을 때, 컨테이너 B 실행이 의미가 있는가
			- 예를 들어 B가 로그수집기이고 A가 애플리케이션이라면 B를 실행하는 것이 의미가 없다. 컨테이너들의 관계가 강한 경우 컨테이너를 Pod 안에 묶을 수 있다.
	2. 스케일링 요구사항이 같은가?
		- 웹 서버 vs 데이터베이스, 트래픽이 많은 vs 그렇지 않은
			- 예를 들어 서로 다른 서비스를 하나에 Pod에 구성한다고 한다면, A라는 서비스는 스케일이 하나면 충분한데, B는 트래픽이 높아서 10개를 실행해야한다. 그렇다면 굳이 A를 묶을 필요는 없다.
			- 웹서버와 DB 같은 경우는 스케일링을 할 때 그 방법도 굉장히 차이가 난다. DB 자체를 스케일링하는 것도 까다롭다. 데이터를 어떻게 복제할 것인지 등의 관점에 대해서
	3. 인프라 활용도가 더 높아지는 방향으로
		-  쿠버네티스가 노드 리소스 등 여러 상태를 고려하여 Pod를 스케일링
			- 파드를 크게 설계할 수록 리소스의 빈 부분을 찾기 어려워질 수 있다. 
- 기본적으로 사실 Pod는 생성과 종료가 빈번하게 이루어지기 때문에 서로 다른 컨테이너를 하나의 Pod로 묶기 보다는 1 : 1로 생성하는 것을 추천한다.

## Pod 배포
### 노드에 배포되는 과정
1. API 서버가 사용자로부터 Pod 배포 요청을 수락한다.
2. Replication Controller가 요청 받은 수 만큼 Pod Replica를 생성한다. (Pod desired state == current state)
	- 현재 레플리카 수를 확인하고 요청에 맞게 레플리카 수를 조정해야 한다는 것을 API서버에 다시 보낸다.
	- pod은 아직 노드에 할당되지 않았기 때문에 pod의 정보에는 노드 정보가 없을 것이다. 스케쥴러는 노드 정보가 없는 pod들만 골라서 이벤트를 수신 받게 된다. 
3. Pod을 배포할 적절한 노드를 선택한다. (nodeSelector)
	- scheduler가 적절한 노드에 배치시키면 좋을지를 결정
4. 워커노드에 pod가 생겨나게 되고 해당 노드에 있는 kubelet은 pod의 이벤트를 수신하게 된다. 
5. kubelet은 container runtime에게 이미지 다운로드를 명령하고 Pod 실행을 준비한다. Pod 상태를 업데이트한다.
6. 이미지를 다운로드하고 컨테이너를 실행한다.
7. kubelet은 컨테이너 상태를 계속 확인하기 위해서 container로 health check를 보낸다. 이후 상태를 API 서버에 보내면서 쿠버네티스 상에서 pod이 정상적으로 동작할 수 있도록 유지하는 역할을 한다.
이러한 과정을 통해서 Pod이 워커 노드에 배포되게 되고, 마스터 노드에 있는 여러 구성요소들과 워커 노드의 구성요소들의 협업을 통해서 원하는 배포 상태를 만들어준다.
![](images/Pasted%20image%2020221104163054.png)

## Pod 오브젝트 표현 방법
```yaml
apiVersion: v1               # Kubernetes API 버전
kind: Pod                    # 오브젝트 타입
meatdata:                    # 오브젝트를 유일하게 식별하기 위한 정보
	name: kube-basic         # 오브젝트 이름
	labels:                  # 오브젝트 집합을 구할 때 사용할 이름표
		app: kube-basic
		project: fastcampus
spec:                        # 사용자가 원하는 오브젝트의 바람직한 상태
	nodeSelector:            # Pod을 배포할 노드
	containers:              # Pod 안에서 실행할 컨테이너 목록
	volumes:                 # 컨테이너가 사용할 수 있는 볼륨 목록
```
- apiVersion: 쿠버네티스 오브젝트를 생성하기 위해서 쿠버네티스가 제공하는 인터페이스의 버전
- kind: 생성하고자 하는 오브젝트의 타입
- metadata: 오브젝트의 식별 정보
	- lable: key-value 쌍으로 정보 저장, 쿠버네티스에서는 lable을 이용해서 많은 기능과 동작이 일어난다.
- spec

### spec.nodeSelector: 노드 선택
```yaml
spec:                        
	nodeSelector:            # Pod을 배포할 노드
		gpu: "true"          # 노드 집합을 구하기위한 식별자(key: value)
```
nodeSelector 에서 맞는 조건의 노드에만 배포를 하게 된다.

### spec.containers
```yaml
spec:                        
	containers:                  # Pod 안에서 실행할 컨테이너 목록
	- name: kube-basic           # 컨테이너 이름
	  image: kube-basic:1.0      # 도커 이미지 주소
	  imagePullPolicy: "Always"  # 도커 이미지 다운로드 정책(Always,IfNotPresent,Never)
	  ports:
	  - containerPort: 80        # 통신에 사용할 컨테이너 포트
```
- image에 expose한 포트를 containerPort라는 속성으로 잘 매핑해야한다. 

```yaml
spec:                        
	containers:                  
	- name: kube-basic           
	  image: kube-basic:1.0      
	  env:                    # 컨테이너에 설정할 환경변수 목록
	  - name: PROFILE         # 환경변수 이름
	    value: production     # 환경변수 값
	  - name: LOG_DIRECTORY
	    value: /logs
	  - name: MESSAGE
		    value: This application is running on $(PROFILE) # 다른 환경변수 참조
```
- 컨테이너에서 사용할 환경변수를 env라는 속성을 사용해 지정할 수 있다.
- 이전에 선언했던 환경변수를 참조할 수도 있다.

```yaml
spec:                        
	containers:                  
	- name: kube-basic           
	  image: kube-basic:1.0      
	  volumeMounts:             # 컨테이너에서 사용할 Pod 볼륨 목록
	  - name: html              # Pod 볼륨 이름
	    mountPath: /var/html    # 마운트할 컨테이너 경로
	- name: web-server
	  image: nginx
	  volumeMounts:
	  - name: html
	    mountPath: /usr/share/nginx/html  # 같은 Pod 볼륨을 다른 경로로 마운트
	    readOnly: true
```

### spec.volumes
```yaml
spec:                        
	containers:                  
	volumes:                   # 컨테이너가 사용할 수 있는 볼륨 목록
	- name: host-volume        # 볼륨 이름
	  hostPath:                # 볼륨 타입, 노드에 있는 파일이나 디렉토리를 마운트 하고자 할 때
		  path: /data/mysql
```
- Pod 볼륨 라이프사이클 = Pod 라이프사이클
- Container에서 볼륨 마운트 방법: volumeMounts 속성
- [지원하는 볼륨 타입](https://kubernetes.io/docs/concepts/storage/volumes/)
- 목적에 맞는 볼륨 선택 (hostPath, gitRepo, ConfigMap, Secret)

## Pod의 한계점
Pod 쿠버네티스에 배포되는 기본적인 단위이긴 하지만 실제로 서비스를 운영하는데 있어서 Pod 오브젝트 하나만으로 운영하기는 어렵다.
1. Pod이 나도 모르는 사이에 종료된 경우
	- self-healing 이 없음, Pod이나 노드 이상으로 종료되면 끝
	- "사용자가 선언한 수 만큼 Pod을 유지" 해주는 ReplicaSet 오브젝트 도입
2. Pod IP는 외부에서 접근할 수 없다. 그리고 생성할 때 마다 IP가 변경된다.
	- 클러스터 외부에서 접근할 수 있는 고정적인 단일 엔드포인트 필요
	- Pod 집합을 클러스터 외부로 노출하기 위한 Service 오브젝트 도입

# 컨테이너로 환경변수 전달
![](images/Pasted%20image%2020221104171650.png)

## 나만의 컨테이너 환경변수 키와 값을 설정
```yaml
spec:
	containers:
	- env:
		- name: STUDENT_NAME
		  value: 하종수
		- name: GREETING
		  value: 쿠버네티스 입문 강의에 오신 것을 환영합니다. $(STUDENT_NAME)님!
```

## Pod 오브젝트 값을 환경 변수 값으로 설정
```yaml
spec:
  containers:
  - env:
    - name: NODE_NAME
      valueFrom:                # '쿠버네티스 오브젝트로부터 환경변수 값을 얻겠다.'
        fieldRef:               # 'Pod spec, status의 field를 환경변수 값으로 참조하겠다'
          fieldPath: spec.nodeName # 참조할 field의 경로 선택
    - name: NODE_IP
      valueFrom:
        fieldRef:
          fieldPath: status.hostIP
```
- Pod이 생성된 이후에 정해지는 값을 환경변수로 사용하고자 한다. 그런 경우에는 valueFrom으로 설정

# Pod과 컨테이너들 간의 통신
![](images/Pasted%20image%2020221104183007.png)
- 하나의 pod에 컨테이너 두 개, localhost 통신
- 다른 pod의 pod ip로 통신

# Label과 Selector

- Label :쿠버네티스 오브젝트를 식별하기 위한 key/value 쌍의 메타데이터
- Selector: Label 을 이용해 쿠버네티스 리소스를 필터링하고 윈하는 리소스 집합을 구하기 위한 **label query**

![](images/Pasted%20image%2020221104185751.png)

## 언제 필요할까?
1. 클러스터에서 서로 다른 팀의 수 백개 Pod이 동시에 실행되고 있는 상황에서 주문 트래픽을 주문 Pod으로, 배달 트래픽을 배달 Pod으로 라우팅 해야할 때
2. 꽃 배달 기능 추가로 배달 트래픽이 증가되는 상황에서 클러스터에서 실행 중인 배달 관련 Pod들을 수평 확장해야 할 때
즉, 우리가 어떤 리소스를 선택해서 명령을 실행하고자 할 때, Label과 Selector를 이용한다.
- Label: 쿠버네티스 리소스를 논리적인 그룹으로 나누기 위해 붙이는 이름표
- Selector: Label 을 이용해 쿠버네티스 리소스를 선택하는 방법(label query)
예를 들어 app: delivery인 pod들만 선택해서 요청을 처리해달라.
이를 사용하기 위해서는 파일을 작성할 때, label을 붙여야 하고 요청을 할 때 selector를 사용해야 한다.

## Label
### label 정의
label은 `metadata.labels` 에서 정의한다.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    name: backend
    version: v1
    env: prod
spec:
  containers:
  - name: myapp
    image: <Image>
```
- my-pod는 세 개의 label을 통해서 특정 지을 수 있게 된다.

### label 확인
```shell
$ kubectl get pod red-app --show-labels
NAME      READY   STATUS    RESTARTS   AGE   LABELS
red-app   1/1     Running   0          44s   <none>
```

### label 추가, 변경, 삭제
```shell
$ kubectl label pod red-app app=backend
pod/red-app labeled
$ kubectl get pod red-app --show-labels
NAME      READY   STATUS    RESTARTS   AGE     LABELS
red-app   1/1     Running   0          2m31s   app=backend
```

```shell
$ kubectl label pod red-app version=v1 
pod/red-app labeled
$ kubectl get pod red-app --show-labels
NAME      READY   STATUS    RESTARTS   AGE    LABELS
red-app   1/1     Running   0          5m1s   app=backend,version=v1

$ kubectl label pod red-app version=v2 --overwrite
pod/red-app labeled
$ kubectl get pod red-app --show-labels           
NAME      READY   STATUS    RESTARTS   AGE     LABELS
red-app   1/1     Running   0          5m18s   app=backend,version=v2
```

```shell
$ kubectl label pod red-app app-               
pod/red-app unlabeled

$ kubectl get pod red-app --show-labels           
NAME      READY   STATUS    RESTARTS   AGE    LABELS
red-app   1/1     Running   0          9m2s   env=prod,version=v2
```

### label key 선택 출력
```shell
$ kubectl get pod red-app --label-columns app,env
NAME      READY   STATUS    RESTARTS   AGE     APP       ENV
red-app   1/1     Running   0          7m31s   backend   prod

$ kubectl get pod red-app -L app,env
NAME      READY   STATUS    RESTARTS   AGE     APP       ENV
red-app   1/1     Running   0          7m47s   backend   prod
```

## Selector
- equality based selector
	- `=`, `!=`
	- key=value: key값이 value일 때
	- key!=value: key값이 value가 아닐 때

- set based selector
	- 값이 어떤 집합에 속해 있다/ 속해 있지 않다.(OR 연산 가능)
		- `key in (value1, value2, ...)`
		- `key notin (value1, value2, ...)`
	- 키가 존재한다/ 존재하지 않는다.
		- `key`
		- `!key`

### get 명령어와 함께 사용하는 경우
```shell
kubectl get <오브젝트 타입> --selector <label query 1, ..., label query N>
kubectl get <오브젝트 타입> -l <label query 1, ..., label query N>
```

# label, selector 실습
![](images/Pasted%20image%2020221104193719.png)

# nodeSelector로 선택한 노드에 배포
![](images/Pasted%20image%2020221104195719.png)
