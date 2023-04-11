이번 프로젝트에서는 Jenkins organization job을 이용하여 CI/CD 파이프라인을 작성했습니다. organization job은 multibranch job의 폴더로 볼 수 있습니다. 그리고 multibranch job은 하나의 repository에서 branch 별로 파이프라인을 모아둔 파이프라인의 폴더로 볼 수 있습니다. 따라서 organization job은 규모가 큰 jenkins 파이프라인들의 집합으로 보시면 됩니다.
multibranch job이나 organization job을 사용하면 좋은 점은 아래 처럼 PR 생성후 merge 전에 파이프라인이 제대로 동작하는지 확인할 수 있게 체크할 수 있다는 점 입니다.

![](images/Pasted%20image%2020230411183510.png)

또한 하나의 job안에 repository들이 정리되어 있기 때문에 각 repository들에 대해서 build가 어떻게 진행됐는지 확인하는 것도 편합니다.

![](images/Pasted%20image%2020230411183733.png)

![](images/Pasted%20image%2020230411183801.png)

![](images/Pasted%20image%2020230411183827.png)

Organization job에 대한 내용은 [링크](../jenkins/organization%20job.md)로 대체하겠습니다.

# 1. github app 생성
1. organization member에게 github app manager 권한 부여
organization에서는 owner만 github app을 관리할 수 있습니다. github app을 관리할 사람을 owner로 추가하는 것 대신 member에게 관련 권한을 부여할 수 있습니다.
owner는 organization > settings > developer settings > github apps > management에서 특정 member를 github app manager로 지정할 수 있습니다. 그러면 해당 member는 해당 organization의 github app을 관리할 수 있는 권한이 생깁니다.
2. 위의 링크에서 github app을 만들었던 것 처럼 organization에서 app을 생성합니다. 
3. organization owner가 해당 github app을 CI/CD를 적용할 repository들에 app을 install 합니다.

# 2. Jenkins Organization job 생성
1. 위의 링크 내용을 참고하여 Organization job을 생성합니다. 그러면 Organization Job은 Repository를 스캔하면서 Jenkinsfile이 존재하는지 확인합니다. 

![](images/Pasted%20image%2020230412001759.png)

2. 그 결과 프로젝트에 Jenkinsfile을 가지고 있는 repository 6개가 조회되었습니다.

![](images/Pasted%20image%2020230412001905.png)

3. repository 별로 확인을 해보면 Branch 별, PR 별로 파이프라인이 빌드 됨을 확인할 수 있습니다.

![](images/Pasted%20image%2020230412002014.png)

4. 사용한 Jenkinsfile의 내용은 다음과 같습니다.
	- 브랜치 별로 분기를 걸어서 브랜치마다 파이프라인이 다르게 동작하게 했습니다. 배포가 필요하지 않은 브랜치의 경우에는 docker image를 push 하지는 않게 했습니다.
	- 배포하는 경우, 바뀐 내용을 Argo CD가 Watch 하고 있는 github repository에 push하도록 했습니다.
```groovy
pipeline {
  agent any
  
  parameters {
    string name: 'IMAGE_NAME', defaultValue: '<REPO-NAME>'
    string name: 'IMAGE_REGISTRY_ACCOUNT', defaultValue: '<DOCKERHUB-ID>'
  }

  stages {    
    stage('Test Gradle Project') {
      steps {
          sh './gradlew test --no-daemon'
      }
    }
    stage('Build Gradle Project') {
      steps {
          sh './gradlew build -x test --parallel --no-daemon'
      }
    }
    
    stage('Build Docker Image') {
      steps {
        sh "docker image build -t ${params.IMAGE_NAME} ."
      }
    }

    stage('Remove Docker Image') {
      when {
        not {
            anyOf {
            branch 'main';
            branch 'be-dev'
            }
        }
      }
      steps {
        sh "docker images ${params.IMAGE_NAME} -q | xargs docker rmi -f"
      }
    }


    stage('Tagging Docker Image') {
      when {
        anyOf {
          branch 'main';
          branch 'be-dev'
        }
      }
      steps {
        sh "docker image tag ${params.IMAGE_NAME} ${params.IMAGE_REGISTRY_ACCOUNT}/${params.IMAGE_NAME}:latest"
        sh "docker image tag ${params.IMAGE_NAME} ${params.IMAGE_REGISTRY_ACCOUNT}/${params.IMAGE_NAME}:${BUILD_NUMBER}"
      }
    }


    stage('Publish Docker Image') {
      when {
        anyOf {
          branch 'main';
          branch 'be-dev'
        }
      }
      steps {
        withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
          sh "docker image push --all-tags ${params.IMAGE_REGISTRY_ACCOUNT}/${params.IMAGE_NAME}"
          sh "docker images ${params.IMAGE_NAME} -q | xargs docker rmi -f"
        }
      }
    }

    stage('Update Kubernetes manifests') {
      when {
        anyOf {
          branch 'main';
          branch 'be-dev'
        }
      }
      steps {
            git branch: 'main', credentialsId: 'cicd-sssdev', url: 'https://github.com/sss-develops/application-manifests.git'
            sh "./change-image-tag.sh ${params.IMAGE_REGISTRY_ACCOUNT} ${params.IMAGE_NAME} ${env.BUILD_NUMBER} ${env.WORKSPACE}"
            withCredentials([gitUsernamePassword(credentialsId: 'cicd-sssdev', gitToolName: 'Default')]) {
              sh "git push origin main"
            }
        }
    }
  }
}
```

# 3. Argo CD 사용
하나의 repository 안에 디렉토리별로 server를 나눠서 각각 Argo CD로 watch하도록 했습니다.

![](images/Pasted%20image%2020230412002727.png)

![](images/Pasted%20image%2020230412002549.png)

