organization job은 multi branch job을 모아놓은 폴더로 볼 수 있다. 따라서 이전보다 훨씬 더 큰 단위이다. 
이번에는 repository 2개를 이용하여 실습을 진행하도록 한다, README.md 파일만 가지고 있는 2개의 repository를 사용한다. 

![](images/Pasted%20image%2020230403154456.png)

# github app 생성하기
이전에 생성해놓은 github app을 그대로 사용하도록 한다. 대신 repository를 micro1과 micro2로 변경해준다. 
1. settings > developer settings > github apps > 앱 선택 > Install App 에서 설정 선택

![](images/Pasted%20image%2020230403154702.png)

2. repository access 에서 repository 를 변경한다.

![](images/Pasted%20image%2020230403154853.png)

3. 이번에는 jenkins controller가 webhook을 사용할 수 있게 permissions & events 에서 webhooks를 read and write로 변경해준다. (settings > developer settings > github apps > 앱 선택 > permissions & events > repository permissions )

![](images/Pasted%20image%2020230403155538.png)

4. settings > developer settings > github apps > 앱 선택 > Install App 에서 설정 선택 에서 바뀐 permission에 대한 업데이트를 수락해준다. 

# github organization job 생성

1. jenkins 대시보드에서 새로운 item을 생성하고 Organization Folder로 선택한다.

![](images/Pasted%20image%2020230403160424.png)

2. Projects > Repository Source는 Github Organization으로 선택한다. 

![](images/Pasted%20image%2020230403160741.png)

3. github에 있는 organization을 사용하므로 API endpoint는 `https://api.github.com` 으로 설정한다.  **Manage Jenkins » Configure System » GitHub Enterprise Servers** 에서 먼저 해당 api server를 추가해놔야한다.  credential은 이전에 생성한 credential을 사용한다.

![](images/Pasted%20image%2020230403161020.png)

![](images/Pasted%20image%2020230403161130.png)

4. 현재 github app에 두 개의 repository만 추가했기 때문에 Behavior에서 포함할 repository를 필터링할 수 있도록 filter by name 을 추가해준다.

![](images/Pasted%20image%2020230403161305.png)

![](images/Pasted%20image%2020230403161346.png)

5. 이후 해당 organization에 있는 repo 중에서 위에서 지정한 repository들에 대해서만 scan을 진행하는 것을 확인할 수 있다. 그리고 각각의 repository에 대해서 동작은 이전 multi branch job에서와 동일하게 모든 브랜치에 대해서 Jenkinsfile을 찾는다. 

![](images/Pasted%20image%2020230403161547.png)

# micro1 repo에 Jenkinsfile push
1. micro1 repository에 다음의 Jenkinsfile을 main branch에 push한다. 

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
❯ git commit -m "ADD Jenkinsfile"
[main 66f1339] ADD Jenkinsfile
 1 file changed, 25 insertions(+)
 create mode 100644 Jenkinsfile
❯ git push origin main
오브젝트 나열하는 중: 4, 완료.
오브젝트 개수 세는 중: 100% (4/4), 완료.
Delta compression using up to 4 threads
오브젝트 압축하는 중: 100% (3/3), 완료.
오브젝트 쓰는 중: 100% (3/3), 484 바이트 | 484.00 KiB/s, 완료.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/Hahajongsoo/micro1
   16bc147..66f1339  main -> main
```

2. jenkins organization job에 multi-branch job이 추가되었음을 확인할 수 있다. organization 폴더 안에 multi-branch 폴더가 생성되고 그 폴더 안에 branch 별로 job이 생성되는 것으로 볼 수 있다. 

![](images/Pasted%20image%2020230403162447.png)

![](images/Pasted%20image%2020230403162504.png)

3. 새로운 브랜치 fix-123 을 생성하면 그에 대한 job이 자동으로 생성됨을 다시 확인할 수 있다.

```sh
❯ git branch fix-123
❯ git switch fix-123
'fix-123' 브랜치로 전환합니다
❯ git push origin fix-123
Total 0 (delta 0), reused 0 (delta 0)
remote: 
remote: Create a pull request for 'fix-123' on GitHub by visiting:
remote:      https://github.com/Hahajongsoo/micro1/pull/new/fix-123
remote: 
To https://github.com/Hahajongsoo/micro1
 * [new branch]      fix-123 -> fix-123
```

![](images/Pasted%20image%2020230403162858.png)

4. PR 생성 및 merge등도 이전과 동일하게 동작한다. 

# micro2 repo에 Jenkinsfile 생성하기
1. micro2 repository에도 마찬가지로 Jenkisfile을 생성한다. 그러나 이번에는 파일 이름을 `Jenkinsfile-m2`로 지정한다.

```sh
❯ cat Jenkinsfile-m2
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
          echo "Hello from Jenkinsfile-m2"
        }
      }
    }
  }
}
❯ git add Jenkinsfile-m2
❯ git commit -m "ADD Jenkinsfile-m2"
[main 9451dcd] ADD Jenkinsfile-m2
 1 file changed, 25 insertions(+)
 create mode 100644 Jenkinsfile-m2
❯ git push origin main
오브젝트 나열하는 중: 4, 완료.
오브젝트 개수 세는 중: 100% (4/4), 완료.
Delta compression using up to 4 threads
오브젝트 압축하는 중: 100% (3/3), 완료.
오브젝트 쓰는 중: 100% (3/3), 499 바이트 | 499.00 KiB/s, 완료.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/Hahajongsoo/micro2
   af7afa6..9451dcd  main -> main
```

2. 이 경우 파일 이름이 정확히 Jenkinsfile이 아니기 때문에 organization job에 micro2 repository가 추가가 되지 않은 것을 확인할 수 있다.

![](images/Pasted%20image%2020230403163823.png)

3. organization job > configure > Projects > Projects Recognizers 에 Jenkinsfile-m2를 추가해준다. 변경사항을 저장하면 scan을 다시 진행하고 micro2 repo가 추가(multi-branch job이)된 것을 확인할 수 있다. 

![](images/Pasted%20image%2020230403164103.png)

![](images/Pasted%20image%2020230403164152.png)

하지만 이렇게 하는 경우 Jenkinsfile을 관리하는데 혼동이 있을 수 있어 좋지 않다. 따라서 Jenkinsfile 하나로만 관리하는 것이 더 좋다. 그리고 만약 하나의 job 안에 Jenkinsfile, Jenkinsfile-m2 둘 다 존재한다면 cofigure의 project recognizer에 Jenkinsfile이 먼저 정의 되어 있어서 Jenkinsfile을 기반으로 파이프라인을 빌드하게 된다.