이번 프로젝트에서는 Jenkins organization job을 이용하여 CI/CD 파이프라인을 작성했습니다. organization job은 multibranch job의 폴더로 볼 수 있습니다. 그리고 multibranch job은 하나의 repository에서 branch 별로 파이프라인을 모아둔 파이프라인의 폴더로 볼 수 있습니다. 따라서 organization job은 규모가 큰 jenkins 파이프라인들의 집합으로 보시면 됩니다.
multibranch job이나 organization job을 사용하면 좋은 점은 아래 처럼 PR 생성후 merge 전에 파이프라인이 제대로 동작하는지 확인할 수 있게 체크할 수 있다는 점 입니다.

![](images/Pasted%20image%2020230411183510.png)

또한 하나의 job안에 repository들이 정리되어 있기 때문에 각 repository들에 대해서 build가 어떻게 진행됐는지 확인하는 것도 편합니다.

![](images/Pasted%20image%2020230411183733.png)

![](images/Pasted%20image%2020230411183801.png)

![](images/Pasted%20image%2020230411183827.png)

Organization job에 대한 내용은 [링크](../jenkins/organization%20job.md)로 대체하겠습니다.



# 1. github app 생성
organization에서는 owner만 github app을 관리할 수 있다. github app을 관리할 사람을 owner로 추가하는 것은 옳지 않은 방법이라 생각된다. 대신 member에게 관련 권한을 부여할 수 있다.
owner는 organization > settings > developer settings > github apps > management에서 특정 member를 github app manager로 지정할 수 있다. 그러면 해당 member는 해당 organization의 github app을 관리할 수 있는 권한이 생긴다.