CI/CD Process
-------------

* Overview

이 문서는 CI/CD Process를 정의하기 위해 작성되었다.

CI/CD 환경 구축을 진행하면서 지속적으로 문서내용을 업데이트 할 예정이다.



1. CI/CD Tools 정의

  - CI : Jenkins 사용

Google Cloud Build가 검토되었으나, Build 이외에 파이프라인 구성이 어려워 제외
  - CD : Jenkins 사용

Terraform, Ansible이 검토되었으나, Jenkins Plugin + gcloud/kubectl Command가 보다 효율적이므로, Jenkins로 진행


  * 검토내용

Terraform : 아래 이슈로 부적합
이슈 1 : Cluster 관련 변경시 전체 Cluster의 Remove and Create가 발생하여 Live Service의 CD 용도로는 부적합함
이슈 2 : GKE Console에서 변경한 내용을 Terraform으로 import시 변경된 설정이 모두 Update 안되는 경우가 있음 
Ansible : Kubernetes 관리용으로는 비효율적인 부분이 많아 제외(참고 : Ansible 사용성 검토)

2. Process 정의

1) CI/CD Process

- CI


Clone Repository
    | Jenkins Pipeline에서 지정한 Git repository에서 Source를 Clone 한다.
     |Git repository에 대한 접근권한에 문제가 있을경우 Pipeline Job이 실패 처리된다.

Build image	
    | Git repository에서 Clone한 Source를 Build 한다.
    | Build가 끝나면 다음 Step을 위해 Docker Image를 실행 한다.

Test image	
    | Build image Step에서 실행된 Docker를 Test한다.
    | 현재는 Test 조건이 작성되지 않았으므로, 필요한 조건을 작성하여 사용한다.
    | Test과정에서 Error가 발생하면 Pipeline Job이 실패 처리된다.
    | Test가 정상적으로 완료되면 Docker를 중지한 후 삭제 처리한다.

Push image	
    | Test가 끝난 Docker Image를 GCP Container Registry에 Push한다.

- CD

Partial Deploy
    | GCP Container Registry에 등록된 Docker Image를 부분 배포한다.
    
Full Deploy	
    | GCP Container Registry에 등록된 Docker Image를 전체 배포한
    
    
3 CI/CD 세부 전략

1) Repository 관리

- GitHub Repository (Source)
    | 각 Repository 별로 staging, Live 서버 배포에 사용할 Branch를 지정한다.
         | * Staging : QA
         | * Live : Release
    | Github Repository의 변경정보를 jenkins로 전달할 수 있도록 Webhook을 설정한다.
    | CI 과정에서 필요한 TestCode용 shell은 해당 Repository 의 최상위 트리에 포함시킨다.
    
- GitHub Repository (Tool)
    | Build 및 Deploy에 필요한 Jenkinsfile 과 shell파일을 저장한다.
    | Jenkinsfile은 CI/CD용 및 Staging/Live용으로 나눠져 있다.
    | (Staging에서는 CI/CD를 구분하지 않고 staging.Jenkinsfile에서 한번에 처리한다.)   
