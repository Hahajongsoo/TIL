# 도커 컴포즈
단일 서버에서 도커 엔진을 사용해서는 도커 커맨드 라인 명령어를 통해서 하나씩 도커 컨테이너를 실행할 수 있었다. 그런데 도커 컴포즈를 사용한다면 다음 처럼 컨테이너들을 사용할 수 있게 된다.
- 단일 서버에서 여러 컨테이너를 프로젝트 단위로 묶어서 관리	
- docker-compose.yml YAML 파일을 통해 명시적 관리
- 프로젝트 단위로 도커 네트워크와 볼륨 관리
- 프로젝트 내 서비스 간 의존성 정의 가능
- 프로젝트 내 서비스 디스커버리 자동화
	- 예를 들어 브릿지 네트워크 모드를 통해 net_alias를 통해 특정 이름으로 컨테이너를 호출 했었는데 도커 컴포즈 내에서는 각각의 서비스 명으로 해당 서비스를 네트워크 상에서 호출할 수 있다. (서비스 디스커버리)
- 손 쉬운 컨테이너 수평 확장

## 프로젝트/서비스/컨테이너
도커 컴포즈를 이해하기 위해서는 세 가지 개념에 대한 이해가 필요하다.

### 프로젝트(Project)
- 도커 컴포즈에서 다루는 워크스페이스 단위
- docker-compose.yml 파일 하나가 프로젝트를 명세한다고 보면 된다.
- 함께 관리하는 서비스 컨테이너의 묶음
- 프로젝트 단위로 기본 도커 네트워크가 생성 됨

### 서비스(Service)
- 도커 컴포즈에서 컨테이너를 관리하기 위한 단위
- scale을 통해 서비스 컨테이너의 수 확장 가능

### 컨테이너(container)
- 서비스를 통해 컨테이너 관리

## docker-compose.yml
도커 컴포즈 파일은 version, services, networks, volumes 총 4개의 최상위 옵션을 가진다.
### 버전 (version)
- 가능한 최신 버전 사용 권장
- 도커 엔진 및 도커 컴포즈 버전에 따라서 사용 가능한 yml의 버전이 달라지므로 호환성 매트릭스를 참조해야한다.
- 이제는 deprecated 되었다. [해당 링크](https://github.com/compose-spec/compose-spec/blob/master/spec.md#compose-file)
- 버전 3부터 Docker Swarm과 호환이 되는데, 이 때문에 특정 옵션이 docker swarm에서 사용되는 것인지 docker compose에서 사용되는 것인지 명확히 알고 있어야한다.

### 서비스 (service)
프로젝트 내 구성되는 여러 서비스들을 서브키를 통해 관리한다.

### 네트워크 (network), 볼륨 (volumes)
프로젝트마다 독립으로 구성되게 된다. 프로젝트 내에서 사용할 도커 볼륨을 볼륨 키에서 정의해서 사용하면 됨. 프로젝트 내에서 사용할 네트워크 목록을 네트워크 키에서 정의해서 사용하면 됨. 네트워크를 정의하지 않아도 default 라는 이름으로 브릿지 네트워크가 생성된다.

## docker-compose 명령어

```shell
# 실행중인 프로젝트 목록 확인
docker compose ls

# 전체 프로젝트 목록 확인
docker compose la -a

# docker run 과 유사하다.
# 이미지를 받아오거나 빌드하고 컨테이너를 실행한다.
docker compose up

# 프로젝트 명을 my-project로 하고 백그라운드 실행
docker compose -p my-project up -d

# 프로젝트 내 컨테이너 및 네트워크 종료 및 제거
docker compose down

# 프로젝트 내 컨테이너, 네트워크 및 볼륨 종료 및 제거
docker compose down -v
```
![](images/Pasted%20image%2020221102150306.png)
- 네트워크와 컨테이너가 생성된 것을 확인할 수 있다.
	- 도커 컴포즈로 프로젝트를  생성하게 되면 브릿지 네트워크가 기본적으로 생성된다. 이름은 build_default로 되었는데 prefix는 프로젝트명이 되고 명시하지 않으면 해당 디렉토리 명이 된다. 서비스의 suffix는 해당 서비스의 컨테이너 순서이다.
### 서비스 확장
```shell
# web 서비스를 3개로 확장
docker compose up --scale web=3
```
### 프로젝트 내에서 사용하는 명령어
```shell
docker compose logs

docker compose events

docker compose images

docker compose ps

docker compose top
```

## 주요 사용 목적
### 로컬 개발 환경 구성
- 특정 프로젝트의 로컬 개발 환경 구성 목적으로 사용
- 프로젝트의 의존성(Redis, MySQL, Kafka 등)을 쉽게 띄울 수 있음

### 자동화된 테스트 환경 구성
- CI/CD 파이프라인 중 쉽게 격리된 테스트 환경을 구성하여 테스트 수행가능

### 단일 호스트 내 컨테이너를 선언적 관리
- 단일 서버에서 컨테이너를 관리할 때 YAML 파일을 통해 선언적으로 관리 가능
- 파일을 통해 변경사항 추적 가능
- 팀원간 협업을 통해 코드 리뷰 가능
- 

# 실습

## 1단계: grafana 구성하기
- grafana 의 3000번 포트는 호스트의 3000번 포트와 바인딩
- grafana의 설정 파일인 grafana.ini는 호스트에서 주입 가능하도록 구성하고 읽기 전용 설정
- grafana의 로컬 데이터 저장 겅로를 확인하여 도커 볼륨 마운트
- grafana의 플러그인 추가 설치를 위한 환경변수 설정
- 로그 드라이버 옵션을 통해 로그 로테이팅하여 로그 데이터가 무한히 쌓이지 않게 함

그라파나는 기본적으로 DB를 사용하는데 기본 값으로 SQLite를 사용하게 된다. sqlite는 파일 형태의 데이터베이스로 로컬 데이터 저장 경로에 저장되게 된다. 하지만 운영환경에서는 그라파나에서 관리하는 대쉬보드도 많아지고 이용하는 사용자 수가 많아짐에 따라서 MySQL을 사용하게 될 것임

## 2단계: Grafana + MySQL 구성하기
- 1단계 요구사항 포함
- grafana.ini를 통해 database 설정을 sqlite에서 MySQL로 변경
- MySQL 컨테이너를 docker compose에 db 서비스로 추가
- grafana 서비스가 db 서비스를 database로 연결하도록 구성
- MySQL의 로컬 데이터 저장 경로 확인하여 도커 볼륨 마운트





