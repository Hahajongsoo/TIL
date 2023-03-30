이번 프로젝트에서는 gradle을 사용하여 자바 프로젝트를 빌드했습니다. CI 파이프라인을 구축하는 과정에서 프로젝트 빌드 시간이 꽤 오래 걸린다고 생각되어 이 빌드 시간을 단축하고자 했습니다. 처음에는 도커로 시간 단축하는 방법을 알아보았고 그 다음에는 젠킨스에서 시간을 단축하는 방법을 생각해봤습니다.
먼저 프로젝트에서의 환경은 다음과 같습니다.
- AWS EKS kubernetes 1.25 
	- 기본 스토리지 클래스로 gp2를 사용하며 aws ebs csi driver를 사용했습니다.
- jenkins 는 노드를 쿠버네티스 파드로 사용하는 jenkins on kubernetes 방식을 사용했으며 설치는 helm으로 진행했습니다. [링크](https://www.jenkins.io/doc/book/installing/kubernetes/#install-jenkins-with-helm-v3)

먼저 도커에서 빌드 시간을 단축할 수 있는 방식을 알아보겠습니다.

# 1. docker 사용
gradle로 빌드를 진행하면 프로젝트에서 필요한 plugin과 dependency를 다운로드 합니다. 로컬에서 진행하는 경우에는 해당 정보들이 캐시로 남아 이후 빌드시 시간이 단축됩니다. 하지만 docker image를 사용하여 컨테이너 안에서 `gradle build` 를 진행하는 경우 해당 컨테이너에는 캐시가 없기 때문에 빌드마다 dependency를 다운로드하여 시간이 걸리게 됩니다. 
일반적인 도커 이미지를 빌드하는 Dockerfile의 형태는 다음과 같습니다.

```dockerfile
FROM gradle:7.6.1-jdk17-alpine 
WORKDIR /app
COPY ./ ./
RUN gradle build --no-daemon
EXPOSE 8071
ENTRYPOINT [ "java", "-jar", "build/libs/config-server-0.0.1-SNAPSHOT.jar" ] 
```

베이스 이미지를 다운로드 하는 시간을 제외하고, 이 경우 매번 빌드마다 42초 정도가 걸리게 됩니다.

```sh
[+] Building 41.9s (9/9) FINISHED                                                                                                         
 => [internal] load .dockerignore                                                       0.0s
 => => transferring context: 2B                                                         0.0s
 => [internal] load build definition from Dockerfile                                    0.0s
 => => transferring dockerfile: 213B                                                    0.0s
 => [internal] load metadata for docker.io/library/gradle:7.6.1-jdk17-alpine            0.6s
 => [internal] load build context                                                       0.0s
 => => transferring context: 11.38kB                                                    0.0s
 => [1/4] FROM docker.io/library/gradle:7.6.1-jdk17-alpine@sha256:0392e0e0c4c839bf97fc446c591e05baa13b96ded7b0c5bf40e7ca57f273adea          0.0s
 => CACHED [2/4] WORKDIR /app                                                           0.0s
 => [3/4] COPY ./ ./                                                                    0.0s
 => [4/4] RUN gradle build --no-daemon                                                  40.2s
 => exporting to image                                                                  1.0s
 => => exporting layers                                                                 1.0s
 => => writing image sha256:08647d489fb842481870d137c12c3b9d26939937edabb4244287ad9a329c0b45                                         0.0s 
 => => naming to docker.io/library/test:0.2
```

도커 이미지를 빌드하는 과정에서는 볼륨 마운트를 사용할 수 없고, gradle 캐시를 COPY나 ADD를 사용하여 옮기는 경우 결국 로컬에서 프로젝트 별로 해당 캐시를 다시 관리해야하기 때문에 번거로운 부분이 있습니다. 이러한 부분을 dependency를 다운로드하는 부분을 도커 이미지 레이어로 만들어 캐싱하는 방법으로 해결할 수 있습니다.

## 1.1 dependency 다운로드 과정을 layer로 캐싱
도커 파일을 다음과 같이 수정합니다.

```dockerfile
FROM gradle:7.6.1-jdk17-alpine as builder
WORKDIR /build

COPY build.gradle settings.gradle /build/
RUN gradle build --no-daemon > /dev/null 2>&1 || true

COPY . /build
RUN gradle clean build --no-daemon

EXPOSE 8071
ENTRYPOINT [ "java", "-jar", "build/libs/config-server-0.0.1-SNAPSHOT.jar" ] 
```

이 경우 dependency와 관련된 파일만 먼저 복사한 후 빌드를 진행하기 때문에 dependency 관련 다운로드는 진행하지만 빌드할 내용이 없기 때문에 빌드에 실패하게 됩니다. 하지만 표준에러를 `/dev/null` 로 보내고 실패한 결과를 true로 만들어버리기 때문에 해당 라인은 성공하게 됩니다. 이 때 dependency를 다운로드하는 부분이 도커 이미지 레이어 캐시로 남기 때문에 이를 계속 사용할 수 있습니다.

```sh
[+] Building 60.0s (12/12) FINISHED                                                                                                       
 => [internal] load build definition from Dockerfile                                     0.0s
 => => transferring dockerfile: 322B                                                     0.0s
 => [internal] load .dockerignore                                                        0.0s
 => => transferring context: 2B                                                          0.0s
 => [internal] load metadata for docker.io/library/gradle:7.6.1-jdk17-alpine             1.5s
 => [auth] library/gradle:pull token for registry-1.docker.io                            0.0s
 => [internal] load build context                                                        0.0s
 => => transferring context: 9.44kB                                                      0.0s
 => CACHED [1/6] FROM docker.io/library/gradle:7.6.1-jdk17-alpine@sha256:0392e0e0c4c839bf97fc446c591e05baa13b96ded7b0c5bf40e7ca57f2                 0.0s
 => [2/6] WORKDIR /build                                                                 0.0s
 => [3/6] COPY build.gradle settings.gradle /build/                                      0.0s
 => [4/6] RUN gradle build --no-daemon > /dev/null 2>&1 || true                         30.4s
 => [5/6] COPY . /build                                                                  0.0s
 => [6/6] RUN gradle clean build --no-daemon                                            27.5s
 => exporting to image                                                                   0.5s
 => => exporting layers                                                                  0.5s
 => => writing image sha256:82f33f27bfe23c564747cf64a704af93dafda443a1b4482f7a9a282ab92fb2a8                                         0.0s 
 => => naming to docker.io/library/test:0.2                                              0.0s
```

```sh
[+] Building 28.2s (11/11) FINISHED                                                                                                       
 => [internal] load build definition from Dockerfile                                     0.0s
 => => transferring dockerfile: 322B                                                     0.0s
 => [internal] load .dockerignore                                                        0.0s
 => => transferring context: 2B                                                          0.0s
 => [internal] load metadata for docker.io/library/gradle:7.6.1-jdk17-alpine             0.6s
 => [internal] load build context                                                        0.0s
 => => transferring context: 11.38kB                                                     0.0s
 => [1/6] FROM docker.io/library/gradle:7.6.1-jdk17-alpine@sha256:0392e0e0c4c839bf97fc446c591e05baa13b96ded7b0c5bf40e7ca57f273adea           0.0s
 => CACHED [2/6] WORKDIR /build                                                          0.0s
 => CACHED [3/6] COPY build.gradle settings.gradle /build/                               0.0s
 => CACHED [4/6] RUN gradle build --no-daemon > /dev/null 2>&1 || true                   0.0s
 => [5/6] COPY . /build                                                                  0.0s
 => [6/6] RUN gradle clean build --no-daemon                                             27.1s
 => exporting to image                                                                   0.3s
 => => exporting layers                                                                  0.3s
 => => writing image sha256:10ccc09b03b90adec58c060a3196d26a7d40b9e204533bf4d59e0753e4fdb959                                         0.0s 
 => => naming to docker.io/library/test:0.3                                              0.0s 
```

이렇게 빌드를 수행하면 dependency를 다운로드 하는 부분을 캐싱하여 다음 빌드 때 그대로 사용하기 때문에 빌드 시간이 30초 아래로 떨어지는 것을 확인할 수 있습니다.

최종적으로는 다음처럼 멀티 스테이지를 사용하여 사용하는 이미지의 크기도 줄이는 방식을 사용할 수도 있습니다.

```dockerfile
FROM gradle:7.6.1-jdk17-alpine as builder
WORKDIR /build

COPY build.gradle settings.gradle /build/
RUN gradle build --parallel > /dev/null 2>&1 || true

COPY . /build
RUN gradle build --parallel

FROM openjdk:17.0-slim
WORKDIR /app

COPY --from=builder /build/build/libs/config-server-0.0.1-SNAPSHOT.jar .

EXPOSE 8071
ENTRYPOINT [ "java", "-jar", "config-server-0.0.1-SNAPSHOT.jar" ] 
```

위처럼 도커 파일을 작성하면 gradle 로 자바 프로젝트를 빌드하고 docker 이미지를 빌드하는 것을 한 번에 할 수 있습니다. 하지만 젠킨스를 이용하면서 파이프라인에 각 단계를 나눠서 각 단계의 정상 동작을 파악하는 것이 필요하다고 생각되어 각 단계를 나누기로 했습니다. 그러면 앞서 이야기한 것 처럼 도커 이미지레이어를 이용한 캐싱을 사용할 수는 없습니다. 또한 젠킨스 노드로 쿠버네티스 파드를 이용하기 때문에 파드 상에서 gradle 캐시를 이용할 방법을 생각해봐야 했습니다.

# 2. jenkins on kubernetes 사용
먼저 젠킨스를 쿠버네티스 위에서 사용하는 경우 쿠버네티스 파드를 젠킨스 노드로 이용합니다. 젠킨스의 Jenkins 관리 > 노드 관리 > Configure Clouds 에서 쿠버네티스 api 서버와 통신하여 쿠버네티스 파드를 노드로 사용하게 설정할 수 있습니다. 쿠버네티스 위에 젠킨스를 설치하고 쿠버네티스 플러그인을 설치해야합니다. helm으로 쉽게 설치할 수 있습니다. [링크](https://www.jenkins.io/doc/book/installing/kubernetes/#install-jenkins-with-helm-v3)

![](images/Pasted%20image%2020230331024511.png)

예를들어 다음과 같은 파이프라인을 빌드하면 해당 파이프라인 stage, step을 실행하는 파드가 뜨는 것을 확인할 수 있습니다.
```groovy
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: ubuntu
    command:
    - sleep
    args:
    - infinity
'''
        }
    }
    stages {
        stage('Main') {
            steps {
                sh 'hostname'
            }
        }
    }
}
```

```sh
❯ kubectl describe pod -n jenkins test1-2-sx0x8-bjfcs-6rx45
Name:                      test1-2-sx0x8-bjfcs-6rx45
...
Containers:
  shell:
    Container ID:  containerd://1eaf8c2328a5447fad6e28143b910ce33be0baf28b6d8d2bcc29e6d4828938ef
    Image:         ubuntu
    Image ID:      docker.io/library/ubuntu@sha256:67211c14fa74f070d27cc59d69a7fa9aeff8e28ea118ef3babc295a0428a6d21
    Port:          <none>
    Host Port:     <none>
    Command:
      sleep
    Args:
      infinity
...
```

해당 빌드의 console output에서도 해당 스펙의 파드가 생성됨을 확인할 수 있습니다.

![](images/Pasted%20image%2020230331025000.png)

## 2.1 파이프라인 작성
파이프라인은 다음과 같이 gradle test, build, docker image build 의 단계로 진행되게 작성했습니다. 하지만 이 경우에는 파드에 캐시가 있지 않기 때문에 매번 빌드가 오래 걸리게 됩니다.

```groovy
pipeline {
  agent {
    kubernetes {
yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: gradle
    image: gradle:7.6.1-jdk17-alpine
    command: ['sleep']
    args: ['infinity']
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    command: ['sleep']
    args: ['infinity']
    volumeMounts:
    - name: registry-credentials
      mountPath: /kaniko/.docker
  volumes:
    - name: registry-credentials
      secret:
        secretName: regcred
        items:
        - key: config.json
          path: config.json
"""
    }
  }
  stages {
    stage('SCM Checkout') {
      steps {
        container('gradle') {
          git branch: 'main', url: 'https://github.com/Hahajongsoo/jenkins-test.git'
        }
      }
    }

    stage('Test Gradle Project') {
      steps {
        container('gradle') {
          sh 'gradle test'
        }
      }
    }

    stage('Build Gradle Project') {
      steps {
        container('gradle') {
          sh 'gradle build -x test --parallel'
        }
      }
    }
    
    stage('Build & Tag Docker Image') {
      steps {
        container('kaniko') {
          sh "executor --dockerfile=Dockerfile \
            --context=dir://${env.WORKSPACE} \
            --destination=hahajong/config-server:${env.BUILD_NUMBER}"
        }
      }
    }

  }
}
```

![](images/Pasted%20image%2020230331025931.png)

빌드시 오래 걸리는 것은 gradle test, build 부분이기 때문에 gradle 캐시를 사용할 수 있으면 될 것이라고 생각했습니다. 그래서 persistentVolume을 이용하여 캐시를 저장하고 이를 파이프라인 빌드마다 마운트하는 것을 적용해봤습니다.

## 2.2 persistentVolumeClaim 사용
먼저 지금 프로젝트에서는 AWS EKS를 사용하고 있고 기본 스토리지 클래스로 gp2를 사용하고 있습니다. 또한 aws ebs csi driver를 사용하고 있기 때문에 persistentVolume에 대한 동적 프로비저닝도 사용할 수 있습니다. 따라서 pvc만 생성해놓고 pv는 따로 생성하지 않는 점을 유의해주세요.

다음과 같이 pvc를 생성합니다. 현재 젠킨스가 jenkins 네임스페이스에 배포되어 있고 젠킨스 노드로 사용되는 파드도 jenkins 네임스페이스에 생성되기 때문에 pvc를 jenkins 네임스페이스에 생성합니다.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: gradle-dependency
  namespace: jenkins
spec:
  resources:
    requests:
      storage: 3Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  storageClassName: "gp2"
```

그리고 파이프라인에서 파드가 볼륨을 마운트 할 수 있도록 agent의 스펙을 다음과 같이 수정합니다.  볼륨을 `/home/gradle/.gradle/caches`에 마운트 합니다. 캐시의 기본 위치는 `$USER_HOME/.gradle/caches` 입니다.

```groovy
pipeline {
  agent {
    kubernetes {
yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: gradle
    image: gradle:7.6.1-jdk17-alpine
    command: ['sleep']
    args: ['infinity']
    volumeMounts:
      - mountPath: /home/gradle/.gradle/caches
        name: cache
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    command: ['sleep']
    args: ['infinity']
    volumeMounts:
    - name: registry-credentials
      mountPath: /kaniko/.docker

  volumes:
    - name: cache
      persistentVolumeClaim:
        claimName: gradle-dependency
    - name: registry-credentials
      secret:
        secretName: regcred
        items:
        - key: config.json
          path: config.json
"""
...
```

빌드 이후 pvc를 확인해보면 jenkins node에서 해당 pvc를 사용하는 것을 확인할 수 있습니다.

```sh
❯ kubectl describe pvc -n jenkins gradle-dependency
Name:          gradle-dependency
Namespace:     jenkins
StorageClass:  gp2
...
Capacity:      3Gi
Access Modes:  RWO
VolumeMode:    Filesystem
Used By:       test-3-bs0cs-w56l0-x5bsk
...
```

그대로 빌드를 다시 진행해보면 이전과 속도 차이가 나는 것을 확인할 수 있습니다. (중간 단계에서 ==70s를 27s로==)

![](images/Pasted%20image%2020230331031127.png)

github에 커밋을 푸쉬하여 변경된 사항으로 자바 프로젝트를 빌드하게 하더라도 이전과 비교하여 빌드 속도가 감소된 것을 확인할 수 있습니다.

![](images/Pasted%20image%2020230331031520.png)

젠킨스 노드로 사용되는 파드는 파이프라인이 완료되고 삭제되지만 pvc는 남아있게 됩니다. 이후 파이프라인 빌드로 인해 다시 생성되는 파드들은 계속 같은 이름의 pvc를 사용하기 때문에 이전 캐시들을 다시 사용할 수 있습니다.

도커 이미지 레이어와 jenkins on kubernetes에서 pvc 마운트를 이용하여 캐시를 계속 사용하는 것으로 빌드 시간을 단축할 수 있음을 확인해봤습니다. 

#### 참고 링크
- https://stackoverflow.com/questions/58593661/slow-gradle-build-in-docker-caching-gradle-build
- https://zwbetz.com/why-is-my-gradle-build-in-docker-so-slow/