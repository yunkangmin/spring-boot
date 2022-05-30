AWS API와 명령행 도구의 기본 개념을 알아보자. 그리고 Node.js용 AWS SDK를   
설치하는 방법, AWS CLI를 설치하는 방법을 설명한다. 또한 Node.js용 AWS SDK를   
이용한 EC2, CloudWatch, ELB, Auto Scaling, S3, CloudFront, DynamoDB, Cloud  
Search, SNS, SES, SQS 예제와 동일한 기능을 AWS CLI로 사용하는 방법에 대해    
알아보자.   

AWS의 가장 큰 장점은 모든 기능을 API로 제어할 수 있다는 것이다. AWS API를   
사용하는 방식은 크게 세 가지가 있다.  
- SDK: 프로그래밍 언어의 라이브러리 형태로 제공되는 SDK를 이용하는 방식이다.  
프로그래밍 언어를 지원하며 현재 개발하고 있는 제품과 완전히 통합할 수 있다.  
   - Android, IOS, JavaScript(웹 브라우저용), Java, .NET, Node.js, PHP,   
   Python, Ruby  
- IDE 도구 키트: 개발 툴에 AWS 제어 기능을 플러그인 또는 확장 프로그램 형태로   
설치할 수 있다.  
   - AWS Toolkit for Eclipse: Eclipse에 설치되는 플러그인이다. Eclipse 안에서  
   AWS 리소스를 제어할 수 있고, Java용 SDK가 포함되어 있다.    
   - AWS Toolkit for Visual Studio: Visual Studio에 설치되는 확장 프로그램이다.  
   Visual Studio 안에서 AWS 리소스를 제어할 수 있고, .NET SDK가 포함되어 있다.  
- 명령행 도구(CLI): Unix/Linux의 터미널, Windows의 명령 프롬프트 또는   
PowerShell에서 사용할 수 있는 명령행 도구이다. 스크립트 형태로 만들면 간단  
하게 자동화를 구현할 수 있다.   
  
SDK, IDE 도구 키트, 명령행 도구는 다음 링크에서 다운로드할 수 있다.   
  
https://aws.amazon.com/ko/tools
  
AWS API를 사용하려면 액세스 키와 시크릿 키가 필요하다. 아직 생성하지   
않았다면 'API와 툴 사용을 위한 액세스 키 생성하기'를 참조하기 바란다.   
  
EC2 인스턴스에서 액세스 키와 시크릿 키 없이 AWS API를 사용하려면 'IAM 역할   
생성하기, IAM 역할을 사용하는 EC2 인스턴스 생성하기'를 참조하기 바란다.  
  
예제 소스 코드는 필자의 GitHub 저장소에서 받을 수 있다.   
  
https://github.com/pyrasis/awsbook
  





















