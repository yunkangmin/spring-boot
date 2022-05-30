## Cache Stampede
레디스를 캐시로 사용할 때 데이터의 갱신을 위해 대부분의 서비스에서는 키에 대해 Expire time(TTL)을 설정한다.  
AWS는 ElasticCache의 캐싱 전략 문서에서 데이터를 최신 상태로 유지함과 동시에 복잡성을 줄이기 위해 TTL을 추가할 것  
을 권장하고 있다.  
  
하지만 대규모 트래픽 환경에서 이 TTL값은 예상치 못한 문제를 발생시킬 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/128307477-9b6baf59-fbd9-4fa0-8cd8-341c9cffc8c0.png)

  
레디스는 캐시로, DB 앞단에서 분산된 서버들의 요청을 받고 있다.  
키가 만료되는 시점을 생각해보자.  
read-through 구조에서 레디스에 데이터가 없을 때 서버들은 직접 DB에 가서 데이터를 읽어와 
레디스에 저장한다.  
키가 만료되는 순간 많은 서버에서 이 키를 참조하는 시점이 겹치게 된다.  
모든 서버들이 DB에 가서 데이터를 질의하는 duplicate read와 그 값을 반복적으로 레디스에 write하는  
duplicate write가 발생하게 된다.  

- 초록색 : 정상적인 응답
- 빨간색 : 레디스 Key miss
- 파란색 : 데이터베이스에 질의  

#### 출처 <https://meetup.toast.com/posts/251>
