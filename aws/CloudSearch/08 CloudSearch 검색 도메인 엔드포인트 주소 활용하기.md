# 정리
- 여기서는 검색 도메인에 인덱스를 생성한 것이고 다음글에서는 CLI를 이용  
하여 데이터를 올린다.  
- 파티션    
파티션 테이블로 생성이 되어 있으면 파티션을 지정하여 조회를 하면 아주   
빠르게 데이터를 조회할 수 있다.   
- Analysis Scheme   
각 언어 특성에 맞는 필드 분석 방식을 정의한다. 언어 선택 후 분석 방식을   
선택한다.    

---

지금까지 AWS 콘솔을 이용해서 CloudSearch를 사용해보았다. 이번에는 검색 도메인의  
엔드포인트 주소를 이용하여 문서를 올리고 검색을 해본다.    

# CloudSearch 인덱스 구조를 설계하고 검색 도메인 생성하기  
검색을 위해 간단한 인덱스 구조를 설계한다(아래는 검색을 위한 데이터). 이름, 주소, 핸드폰 번호,  
직급, 나이 정보를 표현한 문서를 예로 든다. 메모장이나 기타 텍스트 편집기를 열고 다음    
코드와 같이 작성한 뒤 data.json으로 저장한다. 
dats.json   
```
[
  {
    "type": "add",
    "id": "1",
    "version": 1,
    "lang": "ko",
    "fields": {
      "name": "홍길동",
      "address": "서울시 종로구",
      "phone": "010-1234-5678",
      "rank": "대리",
      "age": 27
    }
  },
  {
    "type": "add",
    "id": "2",
    "version": 1,
    "lang": "ko",
    "fields": {
      "name": "이율곡",
      "address": "서울시 성북구",
      "phone": "010-4567-8901",
      "rank": "과장",
      "age": 35
    }
  }
]
```
이와 같은 형식을 SDF(Search Data Format)라고 한다.  
- type: 문서를 추가할 때는 add, 삭제할 때는 delete이다. 문서의 내용을 수정  
하려면 type은 add로 설정하고 수정할 문서의 id를 지정한다. 그리고 version  
숫자를 높이면 된다.  
- id: 각 데이터의 고유값이다. 다른 데이터와 중복될 수 없고, 내용을 수정하거나   
삭제할 때 사용된다. 영문 소문자 a부터 z까지, 숫자 0부터 9까지, `_`(밑줄)   
문자를 사용할 수 있으며 24자리까지이다.   
- version: 문서의 버전이다. 문서의 내용을 수정할 때 사용된다.   
- lang: 문서의 언어이다. 영어는 en, 한국어는 ko를 사용한다.  
- fields: 필드 목록이다. 필드가 하나 이상 포함되어야 한다.  
   - 필드명: 64글자까지이며 영문 소문자, `_`(밑줄) 문자, 숫자를 사용할 수   
   있다. 필드명은 밑줄 문자로 시작할 수 없다.   
   - 필드 자료형  
      - 문자열: "hello"로 표현한다.   
      - 숫자: 1 또는 0.1로 표현한다.
      - 배열: [1, 2, 3] 또는 ["hello", "world"]로 표현한다.  
      
**이 데이터를 기반으로 검색 도메인을 생성한다.** CloudSearch Dashboard를 클릭한 뒤  
Create a New Domain 버튼을 클릭한다.  
  
<img src="https://user-images.githubusercontent.com/33191974/158742430-4b7d9a5b-b6a2-4e2b-860b-14602b38e53a.png" width="50%" height="50%"/>    

CloudSearch 검색 도메인을 생성한다.   
- Search Domain Name: 검색 도메인 이름이다. .(점)은 입력할 수 없다.   
exampledomain2를 입력한다.  
- Desired Instance Type: 검색 도메인이 생성할 검색 인스턴스 유형이다. 설정하지   
않으면 search.m1.small을 사용한다. 기본값 그대로 사용한다.    
- Desired Replication Count: 검색 도메인의 복제 개수이다. 데이터 저장 용량과는   
상관없고, 검색 트래픽이 많을 때 설정한다. 5개까지 설정할 수 있다. 기본값   
그대로 사용한다.  
- Desired Partition Cound: 생성할 검색 파티션 개수이다. 검색 인스턴스 search.  
m2.2xlarge에서만 설정할 수 있고, 10개까지 생성할 수 있다.   
  
설정이 완료되었으면 Continue 버튼을 클릭한다.   

<img src="https://user-images.githubusercontent.com/33191974/158743335-584d5c6a-2bcf-4f6c-a355-46968c644a08.png" width="50%" height="50%"/>   
  
CloudSearch 검색 도메인의 인덱스를 정의한다. 방금 작성한 data.json을 이용한다.  
Analyze sample file(s) from my local machine을 선택하고 파일 선택 버튼을  
클릭한다.  

<img src="https://user-images.githubusercontent.com/33191974/158743590-66c3be3f-1289-4f66-aaa9-5f2e6cb00e11.png" width="50%" height="50%"/>   

파일을 열었으면 Create New Search Domain의 Continue 버튼을 클릭한다.  
  
data.json을 분석하여 인덱스가 생성되었다. Analysis Scheme에서 Address, Name,   
rank를 Korean으로 설정한다. Phone과 같이 숫자로만 된 필드는 Korean으로 설정   
하지 않아도 상관없다. 설정이 완료되었으면 Continue 버튼을 클릭한다.   

<img src="https://user-images.githubusercontent.com/33191974/158743936-2d0be1f8-88c9-43a1-90fc-0765092bd3da.png" width="50%" height="50%"/> 

CloudSearch 검색 도메인의 접근 정책을 설정한다. 데이터 업로드용 엔드포인트  
주소와 검색용 엔드포인트 주소에 대한 정책이다. Search and Suggester service:  
Allow all. Document Service: Account owner only를 클릭한다.   

- Search and Suggester service: Allow all, Document Service: Account owner   
only: 검색은 모두 허용하고, CloudSearch를 생성한 AWS 계정만 데이터를 업로드   
할 수 있다.  

<img src="https://user-images.githubusercontent.com/33191974/158744778-b01cd482-1bfc-41e7-88a3-df2221c4c74d.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158745110-3e63a292-f607-48af-9882-d156c934508b.png" width="50%" height="50%"/>  

CloudSearch 검색 도메인 생성이 시작되었다. OK 버튼을 클릭한다.  

<img src="https://user-images.githubusercontent.com/33191974/158745195-1f41cc7f-c337-45e4-90fd-809bb39dda8b.png" width="50%" height="50%"/>  

CloudSearch 검색 도메인 목록에 방금 생성한 검색 도메인(exampledomain2)이   
추가되었다. 완전히 생성되기까지 약 10분 정도 소요된다.   

<img src="https://user-images.githubusercontent.com/33191974/158745307-a5649264-02f2-492c-8b1b-2a4989d0a050.png" width="50%" height="50%"/>  



































