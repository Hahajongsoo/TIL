mulitbranch job은 pipeline의 폴더이다. 하나의 폴더 안에 여러 파일이 있듯이 하나의 multibranch job에 브랜치 별로 pipe라인을 둘 수 있다. 이는 기본적으로 해당 브랜치에 존재하는 Jenkinsfile로 파이프라인을 빌드한다. 
github에서 multibranch pipeline에서는 인증을 위해 github app을 사용한다. github app을 이용하여 jenkins controller에 인증을 할 수 있다. 

# github app 생성
1. 먼저 settigs > Developer settings로 이동한다. 

![](images/Pasted%20image%2020230403122334.png)


![](images/Pasted%20image%2020230403122401.png)

2. New Github App 클릭 

![](images/Pasted%20image%2020230403122456.png)


3. 사용할 app 이름과 homepage url 그리고 webhook url을 작성한다. homepage url은 따로 생성하지 않아서 github url을 넣어줬다. webhook url의 경우 외부로 노출된 jenkins의 주소를 넣어준다. 로컬에 jenkins가 띄워져있는 경우 터널링을 할 수 있는 ngrok 등의 툴을 이용하도록 한다. 현재 프로젝트에서는 쿠버네티스를 사용하고 있으므로 ingress로 노출시킨 url을 넣어줬다. (EKS를 사용하여 ALB를 Ingress로 사용하고 있고 젠킨스 도메인은 route53에서 ALB를 가리키도록 레코드를 생성해놨다.)

![](images/Pasted%20image%2020230403124208.png)

![](images/Pasted%20image%2020230403124334.png)


4. permissions에서 필요한 권한들을 설정해주도록 한다. repository에 대해서 Checks와 Commit statuses에 Read and write를 Administration, Contents, Metadata, Pull requests에 Read only를 두어 총 6개의 권한을 설정했다. 나머지는 설정하지 않았다.

![](images/Pasted%20image%2020230403130527.png)

5. 받을 이벤트들에 대한 설정을 한다. Check run, Check suite, Pull request, Push, Repository 에 대해서 체크해놨다. 

![](images/Pasted%20image%2020230403130838.png)


6. 다른 유저나 organization에서도 사용할 수 있게 하려면 Any account를 선택해준다. 

![](images/Pasted%20image%2020230403131021.png)

## private key 생성
app 생성후 app 상세 페이지 하단에서 generate a private key로 키를 생성할 수 있다. 키를 생성하면 해당 키는 바로 다운로드가 된다.

![](images/Pasted%20image%2020230403131153.png)


다운로드된 키를 젠킨스에서 사용할 수 있도록 키의 포맷을 바꿔줘야한다. 

```
openssl pkcs8 -topk8 -inform PEM -outform PEM -in key-in-your-downloads-folder.pem -out converted-github-app.pem -nocrypt
```

```
❯ ll jenkins-sssdev.2023-04-02.private-key.pem converted-github-app.pem
-rw------- 1 hajong hajong 1.7K  4월  3 13:18 converted-github-app.pem
-rw-r--r-- 1 hajong hajong 1.7K  4월  3 13:18 jenkins-sssdev.2023-04-02.private-key.pem
```

## install app
생성한 app의 페이지에서 Install App을 선택하여 해당 app을 사용할 공간에 해당 app을 설치하도록 한다. 

![](images/Pasted%20image%2020230403132148.png)


모든 레포지토리에 설치할 수도 있고 특정 레포지토리를 선택하여 설치할 수도 있다.

![](images/Pasted%20image%2020230403132259.png)

# Jenkins controller에 github app credential 추가
1. Jenkins 관리 > Manage Credentials 로 이동한다.

![](images/Pasted%20image%2020230403132803.png)

2. globals를 선택하여 credential을 추가한다.

![](images/Pasted%20image%2020230403132841.png)

![](images/Pasted%20image%2020230403132925.png)

3. kind는 GitHub App으로 선택한다. ID는 위에서 생성한 app의 이름을 넣어준다. App ID는 해당 app의 General 페이지(profile > settings > developer settings > github apps)에서 확인할 수 있다. key에는 위에서 포맷을 변경한 키의 내용을 넣어준다. 

![](images/Pasted%20image%2020230403133443.png)

4. test connection으로 연결을 확인한후 credential을 생성한다. 

# jenkins new item
1. multipipeline 으로 새로운 item을 생성한다.

![](images/Pasted%20image%2020230403143609.png)


2. branch source 는 github으로 선택하여 위에서 생성한 credential을 선택한다.

![](images/Pasted%20image%2020230403143702.png)

3. 사용할 repository를 선택하고 validate를 실행한다.

![](images/Pasted%20image%2020230403143843.png)

4. 이후 저장을 하면 지정한 repository를 scan하여 모든 브랜치를 탐색하고 설정에 지정된 대로 Jenkinsfile이 있는지 확인한다. 현재 지정한 repository의 main 브랜치에는 Jenkinsfile이 없고 dev 브랜치에는 Jenkinsfile이 존재한다.

![](images/Pasted%20image%2020230403144156.png)

5. main 브랜치에 간단한 Jenkinsfile을 push 하여 결과를 확인해보면 push event로 pipeline이 동작하는 것을 확인할 수 있다. github repo에 변경사항이 생기면 github app은 webhook event를 보낸다. 이를 받은 jenkins controller는 pipeline을 빌드하는 것이다. 

```groovy
pipeline {
  agent {
    kubernetes {
yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: ubuntu
    image: ubuntu:18.04
    command: ['sleep']
    args: ['infinity']
"""
    }
  }
  stages {
    stage('Hello') {
      steps {
        container('ubuntu') {
          echo "Hello again!"
        }
      }
    }

  }
}
```

```sh
❯ git add Jenkinsfile
❯ git commit -m "UPDATE Jenkinsfile"
[main ecac632] UPDATE Jenkinsfile
 1 file changed, 1 insertion(+), 1 deletion(-)
❯ git push origin main
오브젝트 나열하는 중: 5, 완료.
오브젝트 개수 세는 중: 100% (5/5), 완료.
Delta compression using up to 4 threads
오브젝트 압축하는 중: 100% (3/3), 완료.
오브젝트 쓰는 중: 100% (3/3), 300 바이트 | 300.00 KiB/s, 완료.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/Hahajongsoo/jenkins-test.git
   697a849..ecac632  main -> main
```

![](images/Pasted%20image%2020230403145323.png)


# 새로운 브랜치에서 PR 생성하기
1. 다음의 Jenkinsfile을 dev 브랜치에 푸쉬한다.

```groovy
pipeline {
  agent {
    kubernetes {
yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: ubuntu
    image: ubuntu:18.04
    command: ['sleep']
    args: ['infinity']
"""
    }
  }
  stages {
    stage('Hello') {
      steps {
        container('ubuntu') {
          echo "Hello again!"
        }
      }
    }
    stage('for the dev branch') {
      when {
        branch "dev"
      }
      steps {
        container('ubuntu') {
          echo "This is dev branch!"
        }
      }
    }
    stage('for the PR') {
      when {
        branch "PR-*"
      }
      steps {
        container('ubuntu') {
          echo "this only runs for the PRs"
        }
      }
    }
  }
}
```

```sh
❯ git switch dev
M       Jenkinsfile
이미 'dev'에 있습니다
❯ git add Jenkinsfile
❯ git commit -m "UPDATE Jenkinsfile"
[dev ce581bd] UPDATE Jenkinsfile
 1 file changed, 45 insertions(+), 73 deletions(-)
 rewrite Jenkinsfile (80%)
❯ git push origin dev
오브젝트 나열하는 중: 5, 완료.
오브젝트 개수 세는 중: 100% (5/5), 완료.
Delta compression using up to 4 threads
오브젝트 압축하는 중: 100% (3/3), 완료.
오브젝트 쓰는 중: 100% (3/3), 540 바이트 | 540.00 KiB/s, 완료.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/Hahajongsoo/jenkins-test.git
   ddf86ad..ce581bd  dev -> dev
```

dev 브랜치로의 push로 인해 pipeline이 빌드되고 

![](images/Pasted%20image%2020230403150250.png)

특정 stage가 실행되고 skip 되는 것을 확인할 수 있다.

![](images/Pasted%20image%2020230403150319.png)

2. github repo에서 PR을 생성한다. 

![](images/Pasted%20image%2020230403150502.png)

![](images/Pasted%20image%2020230403150623.png)

3. Jenkins 대시보드를 확인해보면 생성한 multibranch pipeline인 test1에 대해서 Pull Request에 대한 pipeline이 생성되어 있는 것을 확인할 수 있다.

![](images/Pasted%20image%2020230403151008.png)


4. 오픈된 PR에 의해서 pipeline이 빌드된 것을 확인할 수 있고 해당 item의 이름인 PR-1은 PR 번호에서 온 것을 볼 수 있다.

![](images/Pasted%20image%2020230403151207.png)

![](images/Pasted%20image%2020230403151216.png)


5. merge를 수행하면 (dev 브랜치도 삭제) jenkins 대시보드에서 dev 브랜치와 PR에 대한 item들이 비활성화 된 것을 확인할 수 있다. 그리고 main 브랜치에 push가 되기 때문에 pipeline이 자동으로 빌드된다.

![](images/Pasted%20image%2020230403151554.png)

![](images/Pasted%20image%2020230403151736.png)


![](images/Pasted%20image%2020230403151744.png)

![](images/Pasted%20image%2020230403151846.png)

