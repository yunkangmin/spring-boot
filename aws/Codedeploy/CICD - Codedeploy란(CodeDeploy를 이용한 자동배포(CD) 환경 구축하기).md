## CICD - Codedeploy란? (Codedeploy를 이용한 자동배포(CD) 환경 구축하기)

### Codedeploy란?
Codedeploy는 SourceCode를 운영환경에 자동 배포하는 역할을 수행하는 AWS Service이다.  
즉, CD 지속적 배포 서비스이다.  
  
CodeDeploy의 배포대상은, EC2, ECS, Lambda등 여러가지가 존재하지만 이번 포스팅에서는  
EC2에 배포하는 방법을 알아보도록 하자.  

### Code deploy의 구성
#### 에이전트
EC2에 설치하는 프로그램으로 Codedeploy에서 해당 EC2를 사용할 수 있도록 하는 프로그램이다.  
EC2 이외의 배포환경에는 필요하지 않다.  
  
에이전트는 어플리케이션 계정, 배포기록, 배포스크립트 등을 EC2의 루트 디렉토리에 저장한다.  
Amazon Linux, Ubuntu server, RHEL 인 경우, /opt/codedeploy-agent/deployment-root에 위치한다.  

#### Code Deploy 과정
우선 codedeploy를 이용하기전 우리가 기존 자동화를 하지 않았을 때의 배포 과정은 아래와 같다.
1. 어플리케이션 개발
2. 어플리케이션 테스트 및 빌드
3. 실제 운영환경에 전송
4. 운영환경에 기존 실행중이던 프로그램 종료
5. 새 버전의 프로그램 실행

이 중 우리가 code deploy를 이용해 자동화하고자 하는 부분은 3 ~ 5이다.
![image](https://user-images.githubusercontent.com/33191974/139018776-2188c91e-10c8-4111-b2ac-420915e0e92a.png)  
우리가 구축할 환경은 위 그림과 같다. 가용성 보장을 위해서 배포중 최소 한대 이상의 서비스 가능한 상태를  
유지하기를 원한다. 따라서 EC2 2대를 이용할 것이다.  
- 우선 배포할 소스와 appspec(배포 과정이 정의된 스크립트 경로로 이해하면 된다)를 압축하여,  
  S3 또는 GitHub에 업로드한다.  
- 배포 대상 서버(EC2, Lambda 등)에서 Source가 다운로드 된다.
- 그 다음 같이 압축했던 appspec 파일에 정의된대로, 배포 라이프사이클에 맞추어 스크립트를 실행한다.
- 배포가 완료된다.

...















































