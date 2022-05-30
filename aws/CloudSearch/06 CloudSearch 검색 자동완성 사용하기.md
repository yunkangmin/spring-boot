포털 사이트 검색 엔진 사이트에서 검색어를 입력하면 관련된 단어가 자동완성된다.   
CloudSearch에서도 검색 자동완성(Suggester)기능을 사용할 수 있다. 검색 도메인  
(exampledomain)에서 왼쪽 Suggesters를 클릭한 뒤 Add Suggester 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158734932-0288eba4-d618-4275-b177-07eec54a0e64.png" width="50%" height="50%"/>  
  
자동완성을 생성한다.   
- Name: 자동완성의 이름이다. title을 입력한다. 
- Source Field: 자동완성을 할 필드이다. 영화 제목을 검색할 것이므로 title을   
선택한다.  
- Fuzzy Matching: 자동완성에서 퍼지 알고리즘이다. Low을 선택한다. 
   - None(default): 퍼지 알고리즘을 사용하지 않는다. 입력한 문자열과 정확히   
   일치해야 자동완성 리스트가 나온다.   
   - Low: 입력한 문자열에서 문자 1개가 달라도 자동완성 리스트가 나온다.  
   - High: 입력한 문자열에서 문자 2개가 달라도 자동완성 리스트가 나온다.  
- Sort Expression: 검색결과를 정렬한다. 예) 최고 평점순. 기본값 그대로   
비워둔다.   
  
설정이 완료되었으면 Create 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158735384-179ed4a9-aecb-4493-bab2-373f20a9fe82.png" width="50%" height="50%"/>  
  
CloudSearch 자동완성 목록에 방금 생성한 자동완성(title)이 추가되었다. Status를  
보면 인덱싱이 필요하다고 나온다. 위쪽 Run Indexing 버튼을 클릭하여 데이터를  
다시 인덱싱한다.  
<img src="https://user-images.githubusercontent.com/33191974/158735656-15b651fb-73ba-4588-84bb-861480c1ec75.png" width="50%" height="50%"/>  
자동완성을 위한 인덱스를 생성중이다. 완전히 생성되기까지 약 30분 정도 걸린다.   
자동완성을 위한 인덱스가 생성되지 않으면 검색을 해도 자동완성 기능이 동작하지   
않는다.  
<img src="https://user-images.githubusercontent.com/33191974/158735856-cee7cb10-b872-49cd-aa3a-195f5af26a76.png" width="50%" height="50%"/>   
Status가 Active로 바뀌면 왼쪽 Run a Test Search를 클릭한다. 그리고 No Suggester   
버튼을 클릭하면 자동완성 목록이 표시된다. 방금 생성한 자동완성(title)을   
선택한다. 이제 Search 부분에 incep라고 입력하고 잠시 기다리면 자동완성된  
목록이 표시된다.   
<img src="https://user-images.githubusercontent.com/33191974/158738776-4471bee2-4f22-41ec-8475-610482f766e0.png" width="50%" height="50%"/>   
  
자동완성(title)을 생성할 때 퍼지 알고리즘을 Low로 설정했다. 그래서 문자 하나가   
다르더라도 자동완성 목록에 출력된다. incep에서 p가 다른 Incendies, incep와  
정확히 일치하는 Inception, incep와 d가 다른 Independence Day가 출력된다.   






























