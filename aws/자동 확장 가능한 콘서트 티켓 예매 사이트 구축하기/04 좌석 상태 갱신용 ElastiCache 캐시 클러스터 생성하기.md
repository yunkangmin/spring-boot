Auto Scaling 그룹에 속한 EC2 인스턴스끼리 좌석 상태를 주고 받기 위해 Redis를  
사용하는 ElastiCache 캐시 클러스터를 생성한다.    
  
<img src="https://user-images.githubusercontent.com/33191974/159860470-f6bf5c42-c11b-4b2d-8ffc-5f694a91a5f3.png" width="50%" height="50%"/>
 
  
EC2 인스턴스에서 접근할 수 있도록 'ElastiCache Redis 클러스터 Security Group  
생성 및 설정하기'를 참조하여 ElastiCache 캐시 클러스터에 Security Group을    
설정하기 바란다.   

---

# Redis 메세지 보내기, 받기
일반적인 데이터베이스와는 다르게 레디스는 메시지를 주고, 받는 기능을 제공  
한다. Publish 명령으로 보내고, Subsribe 명령으로 받는다.  
통로는 채널(channel)을 이용한다. 채널은 "SET KEY VALUE"에서 사용하는   
'KEY'와 같은 것으로 생각하면 된다. 
방법은 클라이언트1에서 subscribe channel_name을 실행하고, 클라이언트2에서  
publish channel_name "Message"를 실행하면, 클라이언트 1에 "Message"가  
나온다.  
**레디스의 Pub/Sub 시스템은 메세지를 보관(queuing)하지 않는다. Publish하는  
시점에 이미 실행한 subscribe 명령으로 대기하고 있는 클라이언트에게만  
전달된다.**  


























