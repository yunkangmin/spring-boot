OpsWorks의 기본 개념과 용어 대해 알아보자. 그리고 OpsWorks 스택을 생성하는   
방법, OpsWorks 레이어를 생성하는 방법, OpsWorks 인스턴스를 생성하는 방법, 애플리  
케이션을 배포하는 방법을 설명한다. 또한, 커스텀 Chef 레시피를 사용하는 방법에  
대해 알아보자.   

OpsWorks는 모든 형태의 애플리케이션 구성과 배포를 자동화해주는 서비스이다. 실제로   
서비스를 제공하는 AWS 리소스가 아니라서 사용 요금이 없다.   
  
OpsWorks는 Elastic Beanstalk보다 좀 더 자유도가 높다. Elastic Beanstalk은 미리   
제공되는 애플리케이션 플랫폼만 사용할 수 있지만 OpsWorks는 Chef를 이용하여 어떤   
애플리케이션이든 구성과 배포를 자유롭게 할 수 있다. CloudFormation으로 모든   
부분을 구현하기는 부담스럽고, Elastic Beanstalk보다는 좀 더 세세한 설정을 하고  
싶은 사용자에게 적합하다.   
<img src="https://user-images.githubusercontent.com/33191974/158511691-83fb3631-eef6-4d58-903a-1acbd68c4b30.png" width="50%" height="50%"/>  

> #### DevOps  
> DevOps(Dev + Ops)는 Chef의 개발사인 Opscode에서 만든 용어이다. 보통 개발(  
> Development)과 운영(Operation)은 조직이 분리되어 있고, 업무도 따로 진행된다.   
> 이렇게 되면 업무 효율이 떨어지고, 커뮤니케이션 비용이 발생한다. DevOps는  
> 개발과 운영을 하나의 프로세스로 통합하여 자동화된 시스템을 구축하는 것을   
> 뜻한다.   

OpsWorks에서 제공하는 Chef 레시피는 다음과 같다.   
- Ruby on Rails
- PHP
- Node.js
- Java, Tomcat
- Nginx
- MySQL
- Memcached
- HAProxy: 오픈 소스 TCP/HTTP 로드 밸런서이다.  
- Ganglia: 클러스터 모니터링 시스템이다.   

기본으로 제공되는 Chef 레시피 이외에도 인터넷에 공개된 Chef 레시피를 이용하면   
PostgreSQL, Varnish, CouchDB, Cassandra, MongoDB, Solr, Django와 같은 다양한   
데이터베이스 검색 엔진, 개발 플랫폼 등을 구성할 수 있다.   
  
OpsWorks 구성도   
<img src="https://user-images.githubusercontent.com/33191974/158512142-f2eb8f6b-eb9c-42fd-987a-904990ea9e04.png" width="50%" height="50%"/>    

다음은 OpsWorks 기본 개념이다.   
- 스택(Stack): 스택은 OpsWorks에서 최상위 단위이다. 스택 안에 여러 개의 레이어가   
들어간다. 또한, MySQL 데이터베이스, 로드 밸런서등도 포함된다.   
- 레이어(Layer): EC2 인스턴스 생성, Elastic IP 할당, 애플리케이션을 구성하는   
템플릿이다. Chef 레시피를 이용하여 설치, 업데이트, 배포 작업을 정의한다. 그리고   
로드 기반, 시간 기반 Auto Scaling 설정도 포함된다.   
- 인스턴스(Instance): 레이어에 포함된 EC2 인스턴스이다. 레이어에 정의한 Chef  
레시피대로 애플리케이션이 설치된다.   
- App: 사용자가 작성한 소스의 배포 단위이다. S3 버킷, HTTP 및 Subversion, Git    
저장소를 사용해 배포할 수 있다.   
- 시간 기반(Time-based): EC2 인스턴스가 생성, 시작, 정지되는 날짜와 시간을 정의  
할 수 있다.   
- 로드 기반(Load-based): CPU 사용률, 메모리 사용량 등에 따라 EC2 인스턴스를   
생성하거나 삭제한다.   
- Chef 쿡북(Cookbook): 레시피, 속성, 템플릿, 라이브러리 등의 묶음이다. 쿡북은   
애플리케이션 단위로 구성한다. 쿡북 저장소로 S3 버킷, HTTP, Subversion, Git을  
사용할 수 있다.   
- Chef 레시피(Recipe): 애플리케이션 설치 및 업데이트, 소스 배포 방법이 정의된  
파일이다.   
- 운영체제: Amazon Linux, Ubuntu 12.04 LTS, 사용자가 만든 AMI를 지원한다. 단,   
Windows는 아직 지원하지 않는다.   
OpsWorks와 Chef  
<img src="https://user-images.githubusercontent.com/33191974/158513258-e0c79a77-eec8-44fd-a9c2-d6ea42958ebe.png" width="50%" height="50%"/>  
   
OpsWorks 작업 흐름  
<img src="https://user-images.githubusercontent.com/33191974/158513369-2a221ed5-9039-4f7c-8762-aeaa7fdd42a4.png" width="50%" height="50%"/>  

































