이제 DLQ를 설정하고 테스트를 해보자.  
우선 삭제된 Message가 들어갈 Queue를 새로 하나 만들어보자.  
sqs-study-dlq라는 이름을 가진 standard Queue를 빠르게 만들었다.  
최대 수신수를 5로 해주자.  
![image](https://user-images.githubusercontent.com/33191974/139575499-7bf29787-bde4-4049-8db0-a7eb4e121c68.png)  
sqs-study로 들어가 편집 > 배달 못한 편지 대기열을 설정해준다.  
![image](https://user-images.githubusercontent.com/33191974/139575554-6b635a79-5053-48e8-a0b3-7636471f357d.png)  
![image](https://user-images.githubusercontent.com/33191974/139575574-27aa7948-51f1-42f7-909e-292caecdb037.png)
### 1. sqs-study에서 메시지 전송
![image](https://user-images.githubusercontent.com/33191974/139575645-89a38e33-77cc-44f1-9d09-71283dc8fa4b.png)      
### 2. 메시지 폴링을 반복적을 클릭한다. 
메시지 폴링을 반복해서 누른다.(폴링 진행상황이 가득차면 폴링버튼을 누를 수 있다.)  
![image](https://user-images.githubusercontent.com/33191974/139575665-743e05e2-7258-4d03-a19e-2426f18b962d.png)    
### 3. 메시지 수신수가 10을 넘어가게 되면
메시지 수신수가 10이 넘으면 사용가능한 메시지 수가 0이 되고 dlq에서 메시지 폴링을 누르면 메시지가  
sqs-study에서 이동되어 있다. 
![image](https://user-images.githubusercontent.com/33191974/139575838-4effe5dc-b9e3-4c05-93fd-bacec71f5fb4.png)  
### 4. DLQ로 메시지가 이동된다.  
![image](https://user-images.githubusercontent.com/33191974/139575970-67cc2ee0-a379-4685-bd9b-1f2e8dfa8f8f.png)    
DLQ를 사용할 때, DLQ의 메시지 최대 보관기간은 원래 Queue(Source Queue)의 보관기간보다 길어야 한다.  
Message timestamp가 변경되는 것이 아니기 때문이다.  






