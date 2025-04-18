#Ops , #Docker
도커 명령어가 클라이언트의 역할을 수행한다.
도커 데몬, 도커 엔진이 우분투에 설치되어 있다.  도커 엔진이 설치되어있는 서버를 도커 호스트라고 한다. 호스트에 이미지와 컨테이너를 관리하게 된다. 이미지는 직접 빌드하거나 풀로 리모트에서 가져오는 방법이 있다. 이미지 레지스트리에서 이미지를 풀 하여 로컬에서 사용할 수 있다.

### 도커 이미지와 컨테이너
이미지와 컨테이너는 도커에서 사용하는 가장 기본적인 단위로 이미지와 컨테이너는 1 : N의 관계이다.
- 이미지  
	- 이미지는 컨테이너를 생성할 때 필요한 요소로 컨테이너 목적에 맞는 바이너리와 의존성이 설치되어 있음
	- 여러 개의 계층으로 된 바이너리  파일로 존재
- 컨테이너
	- 호스트와 다른 컨테이너로부터 격리된 시스템 자원과 네트워크를 사용하는 프로세스
	- 이미지는 읽기 전용으로 사용하며 변경사항은 컨테이너 계층에 저장
	- 컨테이너에서 무엇을 하든 이미지는 영향받지 않음
Dockerfile을 build 명령어로 Docker Image로 만들 수 있고 해당 이미지를 run 명령어로 실행시키면 Docker Container가 된다.
- Image vs Container
- Program vs Process
- Class vs Instance

### 도커 이미지 이름 구성

**저장소 이름/이미지 이름:이미지태그** 의 형식이 기본 형식이다.
- 태그를 생략하면 최신 리비전을 가리키는 latest로 인식
- 저장소 이름을 생략하면 기본 저장소인 도커 허브로 인식한다.

### 도커 이미지 저장소
Image Repository: 도커 이미지를 관리하고 공유하기 위한 서버 어플리케이션
- Public: docker hub, quay
- Private: AWS ECR, docker registry


