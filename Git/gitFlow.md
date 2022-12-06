# Git-Flow
## Git-Flow
- 기능이 아닌 방법론이다.
    - 완벽한 방법론이 아니라 개발 환경에 따라  
    수정하고 변경해서 사용하자.

## Git-Flow의 기본적 운영법
- Git-Flow 는 총 5가지의 브랜치를 사용하여 운영한다.
    - master : 기준이 되는 브랜치로 제품을 배포하는 브랜치
    - develop :  개발 브랜치로 개발자들이 이 브랜치를 기준으로 각자 작업한 것을 Merge 하는 브랜치
    - feature : 단위 기능을 개발하는 브랜치로 기능 개발이 완료되면 develop 브랜치로 합치는 브랜치
    - release : 배포를 위해 master 브랜치로 보내기 전에 먼저 QA(품질검사)를 하기 위한 브랜치
    - hotfix : master 브랜치로 배포를 했는데 버그가 생겼을 때 긴급 수정하는 브랜치
    > master 와 develop이 중요한 매인 브랜치이고 나머지는 필요에 의해서 운영하는 브랜치다.

<img src="https://t1.daumcdn.net/cfile/tistory/99CD994C5E69CCF223">
Git-Flow 설명시 많이 쓰이는 이미지

1. 일단 __master__ 브랜치에서 시작한다.
2. 동일한 브랜치를 __develop__에도 생성한다. 개발자들이 이 develop 브랜치에서 개발을 진행한다.
3. 개발을 진행하다가 회원가입, 장바구니 등의 기능 구현이 필요한 경우 A개발자는 develop 브랜치에서 __feature__ 브랜치를 하나 생성해서 회원가입 기능을 구현하고 B개발자도 develop 브랜치에서 __feature__ 브랜치를 하나 생성해서 장바구니 기능을 구현한다.
4. 완료된 feature 브랜치는 검토를 거쳐서 다시 __develop__ 브랜치로 합친다.(Merge)
5. 이제 모든 기능이 완료되면 develop 브랜치를 __release__ 브랜치로 만든다. 그리고 QA(품질 검사)를 진행하면서 보완하고 버그를 고친다.
6. 모든 것이 완료되면 이제 release 브랜치를 __master__ 브랜치와 __develop__ 브랜치로 보낸다. __master__ 브랜치에 버전 추가를 위해 태그를 하나 생성하고 배포를 한다.
7. 배포를 했는데 미처 발견하지 못한 버그가 있다면 __hotfix__ 브랜치를 만들어 긴급 수정 후 태그를 생성하고 바로 수정 배포한다.

## Git Flow의 진행
- react나 bootstrap 같이 규모가 있는 개발을 할 경우는 브랜치 보다는 __Fork__ 와 __Pull requests__ 를 활용하여 구현 한다.
<img src="https://t1.daumcdn.net/cfile/tistory/99E9D24E5E69CCF224">
__Fork__ 는 브랜치와 비슷하지만 프로젝트를 통째로 외부로 복제해서 개발을 하는 방식이다. 그렇게 하면 개발을 해서 브랜치를 머지(Merge)를 바로 하는 것이 아니라 __Pull requsets__ 로 원 프로젝트 관리자에서 머지 요청을 보내면 원 프로젝트 관리자가 __Pull requsets__ 된 코드를 보고 적절하다고 싶으면 그때 그 기능을 붙이는 식으로 개발을 한다.
