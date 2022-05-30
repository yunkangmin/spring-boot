CloudSearch는 기존에 데이터를 가지고 있는 상황에서 사용하는 것을 전제로 하고 있다.   
이번에는 CloudSearch의 기본 기능을 알아보기 위해 AWS에서 제공하는 IMDB 영화 정보   
예제 데이터로 검색 도메인을 생성해보자. 실무에서 사용하기 위해서는 인덱스 구조를   
설계하고 데이터를 올려야 하는데 이 부분은 'CloudSearch 인덱스 구조를 설계하고  
검색 도메인 생성하기'에서 설명한다.  
  
AWS 콘솔로 접속한 뒤 메인 화면에서 App Services의 CloudSearch를 클릭한다.   
서울리전을 사용한다.   
생성한 CloudSearch 검색 도메인이 하나도 없을 때 아래 그림과 같은 페이지가 표시된다.  
Create a new search domain 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158604688-f3b6d0dd-5290-443f-ac27-900ecf453954.png" width="50%" height="50%"/>   
CloudSearch 검색 도메인을 생성한다.  
- Search Domain Name: 검색 도메인의 이름이다. .(점)은 입력할 수 없다. example  
domain을 입력한다.   
- Desired Instance Type: 검색 도메인이 생성할 검색 인스턴스 유형이다. 설정하지  
않으면 search.m1.small을 사용한다. 기본값 그대로 사용한다.  
- Desired Replication Count: 검색 도메인의 복제 개수이다. 데이터 저장 용량과는   
상관없고, 검색 트래픽이 많을 때 설정한다. 5개까지 설정할 수 있다. 기본값 그대로   
사용한다.  
- Desired Partition Count: 생성할 검색 파티션 개수이다. 검색 인스턴스 search.m2.  
2xlarge에서만 설정할 수 있고, 10개까지 생성할 수 있다.  
  
설정이 완료되었으면 Continue 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158605958-fa6ed4a3-611f-4bfe-91f8-8d8d070d3f76.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/158606923-83cedf28-650d-4b55-97ce-9c271c2e22ad.png" width="50%" height="50%"/>   
해석하자면 경고: 원하는 인스턴스 유형, 복제 수 또는 파티션 수를 변경하면 무료   
평가판 한도를 초과하여 서비스 사용량이 증가하고 추가 요금이 발생할 수 있습니다.   
라는 의미이다. ok를 클릭한다.   
  
CloudSearch 검색 도메인의 인덱스를 정의한다. 여기서 선택한 데이터를 분석하여  
인덱스를 정의하게 된다. Use a predefined configuration을 선택한 뒤 기본값  
그대로 IMDB movies(demo)를 선택한다.   
  
- Analyze sample file(s) from my local machine: 사용자의 컴퓨터에 저장된 데이터를   
분석하여 인덱스를 생성한다.  
- Analyze sample object(s) from Amazon S3: S3 버킷에 저장된 데이터를 분석하여  
인덱스를 생성한다.  
- Analyze sample item(s) from Amazon DynamoDB: DynamoDB에 저장된 데이터를 분석  
하여 인덱스를 생성한다.  
- Use a predefined configuration: 미리 정의된 설정을 사용한다.  
- Manual configuration: 다음 페이지에서 사용자가 인덱스를 직접 정의한다.   

<img src="https://user-images.githubusercontent.com/33191974/158608124-83fe4e41-5262-4122-90bf-4918b4c380e6.png" width="50%" height="50%"/>      
  

AWS에서 제공하는 IMDB 영화 정보를 분석하여 인덱스가 생성되었다. 데이터베이스에서  
테이블을 정의하는 것처럼 검색 엔진도 인덱스를 정의해야 한다. 기본값 그대로 사용  
한다.  
- Search: 항목이 검색되도록 하는 옵션이다.  
- Default Value: 항목의 기본값이다. 데이터를 추가했을 때 내용이 없으면 여기에  
설정한 값을 사용한다.  
- Source Field: 데이터를 복사했을 때 내용을 가져올 항목(데이터 값)이다. 자료형이   
호환(숫자는 숫자끼리, 문자열은 문자열끼리)되는 항목만 사용할 수 있고, 여러 개를   
설정할 수 있다. Source Field로 사용되고 있는 항목은 다른 곳에서 다시 Source   
Field로 설정할 수 없다.  
- Remove: 항목을 삭제한다.   
- Add Index Field: 항목을 추가한다.  
- Re-configure Index: 뒤 페이지로 돌아가서 분석할 데이터를 다시 설정한다.   
  
<img src="https://user-images.githubusercontent.com/33191974/158610215-bcb625b2-1f03-4b08-a815-8831d84aae93.png" width="50%" height="50%"/>    
  
CloudSearch 검색 도메인의 접근 정책을 설정한다. **데이터 업로드용 엔드포인트 주소와   
검색용 엔드포인트 주소에 대한 정책이다.** Search and Suggester service: Allow all.  
Document Service: Account owner only를 클릭한다.   
- Search and Suggester service: Allow all, Document Service: Account owner only:  
검색은 모두 허용하고, CloudSearch를 생성한 AWS 계정만 데이터를 업로드할 수 있다.   
- Allow everyone access to all services: 검색, 데이터 업로드를 모두 허용한다.   
아무나 데이터를 올릴 수 있으므로 권장하지 않는다.   
- Deny everyone access to all services: 검색, 데이터 업로드 모두 차단한다. AWS  
콘솔에서만 데이터를 올리고 검색할 수 있다.   
  
설정이 완료되었으면 Continue 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158611705-8ce38ed9-4478-47ad-9bb2-fab7275c0cf1.png" width="50%" height="50%"/>  
지금까지 설정한 내용에 이상이 없는지 확인한다. 이상이 없으면 Confirm을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158611922-159ea7bc-a0b5-429b-80e7-bd9c9141d0d2.png" width="50%" height="50%"/>  
CloudSearch 검색 도메인 목록에 검색 도메인(exampledomain)이 생성되었다. 완전히   
생성되기가지 약 10분 정도 소요된다.  
  
CloudSearch 검색 도메인 생성이 완료되었다. 검색 도메인(exampledomain)의 세부   
내용에 검색 엔드포인트 주소, 문서 엔드포인트 주소가 표시된다. 엔드포인트 주소에   
HTTP 메서드를 이용하여 데이터를 올리거나 검색 요청을 할 수 있다.   
  
<img src="https://user-images.githubusercontent.com/33191974/158616236-196f5892-3f23-4b9e-81ab-b3ee60dceb88.png" width="50%" height="50%"/>   
    
아직 생성된 검색 인스턴스 개수가 0개라고 표시된다. 1분 저도 기다린 후 Refresh  
버튼을 클릭하면 생성된 검색 인스턴스가 표시된다.   
  
> #### 30일 무료 평가 프로그램  
> 첫 검색 도메인을 생성하면 30일 무료 평가 프로그램이 시작된다. 그리고 검색   
> 도메인을 하나 더 만들면 무료 사용 시간이 2배로 소모된다(검색 인스턴스 유형과   
> 데이터 업로드 용량, 검색 요청량에 따라 달라질 수 있다).  

> #### CloudSearch 검색 도메인이 생성된 뒤 설정할 수 있는 기능  
> - Multi-AZ: 장애가 발생해도 다른 가용 영역에서 서비스를 제공할 수 있도록  
> Multi-AZ 기능을 사용한다.  
>    1. 검색 도메인에서 Availability Options를 클릭한다.   
>    2. Turn Multi-AZ on 버튼을 클릭한다.  
> - 검색 도메인 접근 정책 수정: 검색 도메인을 생성할 때 설정했던 접근 정책   
> 설정방식과 동일하다.  
>    1. 검색 도메인에서 Access Policies를 클릭한다.  
>    2. 접근 정책을 수정한다.
>    3. Submit 버튼을 클릭한다.  
> - 인덱스 구조 변경: 검색 도메인을 생성할 때 설정했던 인덱스 설정 방식과  
> 동일하다.  
>    1. 검색 도메인에서 Indexing Options를 클릭한다.  
>    2. 각 필드의 설정을 변경한다.
>    3. Submit 버튼을 클릭한다.  
> - 자동 확장 설정 변경: 검색 도메인을 생성할 때 설정했던 자동 확장 설정과   
> 동일하다. 
>    1. 검색 도메인에서 Scaling Options를 클릭한다. 
>    2. 검색 인스턴스, Replication, Partition 설정을 변경한다.  
>    3. Submit 버튼을 클릭한다.  
> - Analysis Scheme 추가: 각 언어 특성에 맞는 필드 분석 방식을 정의한다.  
>    1. 검색 도메인에서 Analysis Schemes를 클릭한다.  
>    2. Add Analysis Scheme 버튼을 클릭한다.  
>    3. Analysis Scheme Name을 입력하고, Analysis Scheme Language를 선택한다.  
>    4. Stopwords(불용어), Stemming(형태소 분석), 동의어(Synonyms)를 추가한다.   
>    5. Create 버튼을 클릭한다.  


































