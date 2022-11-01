  
# 이미지 빌드
## 도커 이미지 구조
![](images/Pasted%20image%2020221101160042.png)
- nginx 는 ubuntu 이미지를, web app은 nginx 이미지를 기반으로 빌드되었다고 가정
- 도커 이미지는 레이어 구조로 되어있다. 이전 레이어 위에 다른 레이어가 쌓이는 구조
- 컨테이너를 실행하면 readonly layer인 이미지 레이어 위에 read write 레이어인 컨테이너 레이어가 쌓이는 형태로 실행이 된다.
- `docker image inspect [image]` 로 image의 상세 정보를 확인할 수 있으며 root file system 키에서 레이어들을 확인할 수 있다.
![](images/Pasted%20image%2020221101160600.png)
- 각각의 레이어들은 sha256 으로 해싱된 해쉬값들로 표현된다.

## Dockerfile 없이 이미지 생성
기존 컨테이너를 기반으로 새 이미지를 생성할 수 있다.
```shell
# docker commit [OPTIONS] [CONTAINER] [REPSOTORY[:TAG]]
# ubuntu 컨테이너의 현재 상태를 my_ubuntu:v1 이미지로 생성
# -a : author -m : message
docker commit -a fastcampus -m "First Commit" ubuntu my_ubuntu:v1
```
- 변경 점을 저장하는 명령어로 기존 컨테이너를 기반으로 해당 컨테이너에서 변경사항을 만들고 나서 새로운 이미지로 커밋을 할 수 있다.
- 새로운 레이어가 쌓이고 새로운 이미지가 빌드 된 것을 확인할 수 있다.
![](images/Pasted%20image%2020221101161321.png)
- 그리고 해당 레이어가 원래 이미지 위에 쌓인 것을 inspect로 확인할 수 있다.
- `docker image inspect my_ubuntu:v1`
![](images/Pasted%20image%2020221101161654.png)
- `docker image inspect ubuntu:focal`
![](images/Pasted%20image%2020221101161710.png)

## Dockerfile 이용하여 이미지 빌드
Dockerfile을 기반으로 새 이미지를 생성할 수 있다.  도커파일의 문법은 `[지시어] [지시어에대한 인수]` 로 구성된다. 그리고 지시어들이 순차적으로 실행된다.
```Dockerfile
FROM node:12-alpine
RUN apk add --no-cache python3 g++ make
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```

```shell
# docker build [OPTIONS] PATH
# ./ 디렉토리를 빌트 컨텍스트로 my_app:v1 이미지 빌드 (Dockerfile 이용)
docker build -t my_app:v1 ./

# ./ 디렐토리를 빌드 컨텍스트로 my_app:v1 이미지 빌드 (example/MyDockerfile 이용)
docker build -t my_app:v1 -f example/MyDockerfile ./
```
- 기본적으로는 현재 디렉토리의 도커파일로 빌드를 하지만 다른 도커파일을 기반으로 빌드를 해야하는 경우에  `-f` 옵션을 사용할 수 있다.
### 예시 (node.js 기반 프로젝트)
![](images/Pasted%20image%2020221101162818.png)
![](images/Pasted%20image%2020221101162906.png)
- build에 대한 출력으로 중요한 정보들을 확인할 수 있다.![](images/Pasted%20image%2020221101163104.png)
	- 빌드 컨텍스트를 Docker daemon에 보낸다.
	- 총 6개의 step이 있고 이는 지시어의 수와 같다.

- 내용을 수정하고 build 하는 경우 캐시를 사용하는 것을 확인할 수 있다.![](images/Pasted%20image%2020221101163540.png)
	- 모든 명령어가 캐시를 사용하는 것은 아니지만 도커 데몬이 판단을 했을 때, 이전 이미지 빌드에서 재사용 가능한 레이어는 기존 레이어 값을 가져와 동일한 동작을 수행하지 않게 한다.

## 빌드 컨텍스트
도커 빌드 명령 수행 시 현재 디렉토리(Current Working Directory)를 빌드 컨텍스트(Build Context)라고 한다. Dockerfile로부터 이미지 빌드에 필요한 정보를 도커 데몬에게 전달하기 위한 목적이다.
- 현재 디렉토리에 많은 파일과 디렉토리, 여러 정보들이 있다. 도커 데몬이 빌드 명령어를 받았을 때 빌드 컨텍스트를 받는 것으로 이미지 빌드를 정상적으로 수행할 수 있다. 예를 들어 COPY 명령어 같은 경우 특정 위치의 파일을 복사하는 지시어인데 빌드 컨텍스트를 기준으로 하므로 빌드 컨텍스트를 넘겨주어야 한다.
- 따라서 빌드 컨텍스트를 이해하는 것이 중요하다. 도커 빌드 명령어를 수행할 때, 해당 디렉토리의 모든 정보가 도커 데몬에 넘어간다. 현재 디렉토리 안에 있는 데이터가 너무 크다면 도커 빌드를 수행하는데 시간이 오래 걸리고 비효율적이 된다.
- 이러한 문제를 해결하기 위해 `.dockerignore` 를 제공한다.

### .dockerignore
`.gitignore` 와 동일한 문법을 가지고 있다. 특정 디렉토리 혹은 파일 목록을 빌드 컨텍스트에서 제외하기 위한 목적을 가진다.

# Dockerfile 문법
https://docs.docker.com/engine/reference/builder/

## Format
- 주석을 사용하는 법과 기본적인 형식에 대해서 안내하고 있다.
- 주석은 `#` 을 사용한다.
- 도커파일의 기본 형태는 `INSTRUCTION arguments` 로 구성되어 있다.

## Environment replacement
```Dockerfile
FROM busybox
ENV FOO=/bar
WORKDIR ${FOO}   # WORKDIR /bar
ADD . $FOO       # ADD . /bar
COPY \$FOO /quux # COPY $FOO /quux
```
- 다른 지시어에서 환경변수를 사용하는 것을 확인할 수 있는데, 이는 컨테이너의 환경변수이다.
- 이미지 빌드 단계에서 `ENV` 지시어를 사용하면 이미지 빌드 타임과 컨테이너 런타임에 환경변수 값을 전달할 수 있다.

## ARG
build argument를 전달하는 방법도 있다.
- 값을 사용하는 방법에는 키 밸류로 해당 값을 지정하여 defalut value를 설정할 수 있다.
- `docker build --build-arg [arg]` 를 사용하여 build argument를 전달할 수도 있다.
- build argument를 사용하는 경우에는 scope에 항상 유의해야한다. 정의하기 이전에 사용하면 해당 arg를 사용할 수 없고 기본값을 사용하게 된다.

### ENV, ARG
ENV와 ARG는 동일한 형식을 가지고 있기 때문에 변수가 겹칠 수 있다. 이러한 경우에는 항상 ENV 지시어가 ARG 지시어를 덮어쓰게 된다. 

## 이외 지시어
```Dockerfile
#
# nodejs-server
#
# build:
#   docker build --force-rm -t nodejs-server .
# run:
#   docker run --rm -it --name nodejs-server nodejs-server
#

FROM node:16
LABEL maintainer="FastCampus Park <fastcampus@fastcampus.com>"
LABEL description="Simple server with Node.js"

# Create app directory
WORKDIR /app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]
```

## FROM
어떤 Dockerfile이든 FROM 지시어를 통해서 도커파일을 시작하게 된다. FROM 지시어는 어떤 베이스 이미지를 사용할지 결정하는 지시어이다.

## LABEL
이미지의 메타데이터 설정이다. 기본적으로 optional이므로 사용하지 않아도 되지만 추후에 컨테이너로 서비스를 관리하는게 복잡해지고 관리하는 이미지가 많아지게 되면 LABEL값을 조직내 컨벤션을 가지고 관리하면 좀 더 깔끔하게 이미지와 컨테이너를 관리할 수 있게 된다.

## WORKDIR
다음에 오는 경로를 워킹디렉토리로 만들어준다. cd 명령어를 통해서 디렉토리 이동을 한다고 봐도 된다. 

## COPY
`COPY soruce destination` 의 형태를 띈다. 호스트 운영체제의 경로에서 이미지 상에서 경로로 파일을 복사한다.

## RUN
도커 이미지 빌드상에서 해당 명령어를 실행하라는 것. 

## EXPOSE
뒤에 포트 넘버를 가져오게 된다. 해당 이미지가 특정 번호의 포트를 사용한다고 문서화 하는 것이다. expose를 한다고 해서 해당 포트가 publish 되지는 않는다.

## CMD
해당 이미지를 가지고 컨테이너를 실행할때 어떤 명령어를 실행할 지 결정한다. 컨테이너의 주요 프로세스를 결정한다. 배열 형태로도 받을 수 있고 하나의 문자열 형태로도 받을 수 있다.

## ENTRYPOINT
커맨드 처럼 배열로 전달할 수 있고 쉘 형태로 전달할 수도 있다.

## ADD
copy 지시어와 거의 동일한 역할을 수행한다. ADD 지시어의 경우에는 소스 디렉토리로 url 또한 받을 수 있게 된다. url을 소스 값으로 사용하게 되면 소스값이 변경 됐는지 변경되지 않았는지 확인하기 힘들다.
그냥 copy로 통일하는게 좋아보인다고 함

## USER
도커 컨테이너가 사용하게 될 기본 사용자를 지정할 수 있고, 그룹도 지정할 수 있다. 도커 이미지 보안을 고려할 때 반드시 사용하는 지시어이다.

# 이미지 압축파일로 저장 및 불러오기
인터넷이 안되는 환경에서 활용할 수 있다. 이미지 파일을 온라인 상에 올리지 않고 특정 서버나 사람에게 전달하는 목적으로도 사용할 수 있다.
```shell
# docker save -o [OUTPUT-FILE] IMAGE
# ubuntu:focal 이미지를 ubuntu_focal.tar 압축 파일로 저장
docker save -o ubuntu_focal.tar ubuntu:focal

# docker load -i [INPUT-FILE]
# ubuntu_focal.tar 압축 파일에서 ubuntu:focal 이미지 불러오기
docker load -i ubuntu_focal.tar
```

# 이미지 경량화 전략
도커 이미지의 크기가 작아질수록 도커 이미지를 푸쉬하는 속도도 빨라지고 풀 하는 속도도 빨라진다. 해당 이미지로 컨테이너를 띄우는 속도도 빨라진다. 해당 호스트에서 같은 용량대비 보유할 수 있는 도커 이미지의 양도 늘어난다. 

![](images/Pasted%20image%2020221101175007.png)
## 꼭 필요한 패키지 및 파일만 추가
컨테이너는 하나의 프로세스를 실행하는데 초점을 맞춰 설계된 기술이기 때문에 해당 프로세스를 실행하는데 필요없는 패키지나 파일은 모두 제거하는 것이 좋다
- 도커 이미지 상에서는 패키지에 대한 캐시를 필요로 하지 않는다. 캐시를 남겨두게 되면 이미지의 용량이 크게 증가하게 되므로 no-cache 옵션이 있는지 확인하고 불필요한 패키지를 삭제하고 캐시를 남기지 않는 것을 염두에 둬야한다.

## 컨테이너 레이어 수 줄이기
지시어 수를 줄이기. RUN 명령어를 사용할 때 하나의 RUN 지시어로 줄이는 것을 주로 사용한다.
- 위 이미지에서 RUN 한 줄로 처리한 것 확인 가능

## 경량 베이스 이미지 선택
- 같은 node 이미지여도 베이스 이미지에 따라서 용량 차이가 큰 것을 확인할 수 있다.
![](images/Pasted%20image%2020221101175602.png)
![](images/Pasted%20image%2020221101175926.png)
- debian 의 slim 계열
- alpine 이라는 리눅스 디스트리뷰션
- stretch 라는 파일 시스템만 존재하는 비어있는 이미지. 바이너리를 컴파일 하고 스트레치 이미지에 복사하고 사용하는 용도로 사용한다.


## 멀티 스테이지 빌드 사용
도커의 기능인 멀티 스테이지 빌드를 사용한다. 빌드 스테이지와 릴리즈 스테이지를 나눠서 빌드 때 필요한 빌드 의존성은 빌드 스테이지에서 진행을 하고, 릴리즈 스테이지에서는 빌드 결과문만 복사를 해서 릴리즈 이미지의 용량을 경량화 할 수 있다.
- FROM 지시어에서 AS 를 사용하는 문법이다. 해당 도커파일의 내용을 블록으로 잡고 나서 AS를 통해서  이름을 임시로 부여한다.
![](images/Pasted%20image%2020221101185116.png)
- 나머지 블록들이 base 블록을 베이스로 하는 것을 확인할 수 있다. 즉 base에 정의된 레이어를 재사용하겠다는 의미이다. 해당 레이어위에 다음 레이어를 쌓는 것이다.
	- build에서는 `package*.json`  을 기준으로 node.js 패키지를 실행한다.
	- release 에서는 `COPY --from=build` 에서 build 스테이지에서 copy 해오는 것을 확인할 수 있다. 빌드스테이지에서 빌드된 node.js 패키지들을 release 스테이지로 복사해온다. 이후에 app 소스코드를 복사해온다.
- 빌드 의존성에 따른 용량을 줄일 수 있게 해준다.