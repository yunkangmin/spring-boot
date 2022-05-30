FastGlacier에서 다운로드할 파일을 선택하고, Download 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158057615-c526fd04-4f59-474b-ade1-a05a35729b41.png" width="50%" height="50%"/>    

> #### Glacier 아카이브 목록   
> 이번 실습에서는 FastGlacier로 방금 파일을 올렸기 때문에 FastGlacier에서 파일목록을 저장하고 있다.   
> 다른 Glacier 볼트에서 아카이브를 다운로드하려면 볼트 인벤토리(아카이브 목록)를 먼저 가져와야 한다.   
> 그러므로 파일을 다운로드하려면 아카이브 목록을 가져오는 데 3 ~ 5시간, 파일 반출 요청에 3 ~ 5시간이   
> 소요된다.  

FastGlacier에서 파일 반출 요청을 한다. Request all files immediately를 선택하고 Confirm data retrieval  
버튼을 클릭한다.   

- Request all files immediately: 모든 파일을 즉시 반출하도록 요청한다. 파일의 용량이 작을 때 사용한다.   
반출 작업은 3 ~ 5시간 가량 소요된다.   
- Limit retrieval rate to N MB per hour: 시간당 반출 용량을 제한하여 요청한다. 파일의 용량이 매우  
클 때 사용하면 반출 요청 요금을 줄일 수 있다. 반출 작업은 설정한 용량과 시간에 따라 달라진다.   
<img src="https://user-images.githubusercontent.com/33191974/158057858-c1e3e4b2-6061-4283-af92-ccd4ec5af74c.png" width="50%" height="50%"/>   
FastGlacier에서 다운로드할 파일이 저장될 위치를 지정한다. 필자는 바탕화면으로 지정했다.   
FastGlacier의 Tasks 탭을 보면 파일 반출 요청을 한 뒤 다운로드를 대기하고 있다. FastGlacier에서는  
기본 대기시간이 4시간으로 설정되어 있고, 4시간 뒤에 파일을 자동으로 다운로드한다. FastGlacier를  
종료하더라도 4시간 뒤에 다시 켜면 반출 작업이 완료된 파일을 자동으로 다운로드한다(현재는 몇분뒤  
다운로드됨).        
<img src="https://user-images.githubusercontent.com/33191974/158057964-517446f8-80bc-4395-bb70-a5958db128fa.png" width="50%" height="50%"/>     
필자는 오후 8시 49분쯤에 파일 반출 요청을 했다. 약 4시간 뒤에 메일함에 아카이브 반출이 완료되었다는   
메일이 도착했다. 그리고 약 10분 뒤에 FastGlacier에서 파일 다운로드가 완료되었다(현재는 메일이 안옴).  

> #### FastGlacier와 반출 작업 완료   
> FastGlacier는 반출 작업이 완료되었는지 알 수 없기 때문에 4시간 뒤에 다운로드를 시도해보고, 다운로드에  
> 실패하면 10분 뒤에 계속 재시도한다.   

> #### 아카이브 범위 반출 요청   
> 일반적으로 반출 요청을 하면 아카이브(파일)를 통째로 가져오게 된다. 하지만, 아카이브 범위 반출(Range  
> Retrieval)요청을 하면 아카이브의 일부분만 반출하고 다운로드할 수 있기 때문에 반출 요청 용량과   
> 데이터 전송 비용을 줄일 수 있다.   


































