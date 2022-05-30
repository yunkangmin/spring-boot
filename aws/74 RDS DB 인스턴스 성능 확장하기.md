DB의 사용량이 많아져 부하가 늘어난다면 DB 인스턴스의 성능을 확장해야 한다.  
사양을 높여 성능을 확장하는 것을 수직 확장(Verical Scaling) 또는 Scale up이라고  
한다.   
- DB 인스턴스 클래스: 상위 단계 클래스로 변경하여 vCPU와 메모리 용량을 늘릴 수 있다.   
또한, 네트워크 성능도 향상시킬 수 있다.   
- 스토리지 용량: 3072GB(3TB)까지 저장 용량을 늘릴 수 있다.  
- Provisioned IOPS: IOPS 값을 높여 I/O(읽기/쓰기) 성능을 향상시킬 수 있다.   

> #### Provisioned IOPS   
> Provisioned IOPS는 스토리지 용량이 100GB 이상일 때 사용할 수 있다.  

RDS DB 인스턴스(exampledbinstance)의 성능을 확장해보자. DB 인스턴스 목록에서  
DB 인스턴스(exampledbinstance)을 선택한 뒤 Modify를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156983604-c70bf6a1-6296-4b89-871b-d62b5770730a.png" width="50%" height="50%"/>       
RDS DB 인스턴스의 설정을 변경하여 성능을 확장시킨다.  
- DB Instance Class: DB 인스턴스의 클래스이다. db.m1.small을 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156983892-bc2a49ac-3fcc-47c2-aabb-4dc58e255045.png" width="50%" height="50%"/>   
    
- Allocated Storage: 스토리지 용량이다. 10으로 설정한다. 단, 용량을 줄일 수는   
없다.   
  <img src="https://user-images.githubusercontent.com/33191974/156983986-8c3cfd30-0e58-4281-abaa-99ad23c67e34.png" width="50%" height="50%"/>  
  
- Apply Immediately: 설정 변경 내용을 즉시 적용한다. 이 부분을 체크하지 않으면   
다음 유지 관리 시간(Maintenance Window)에 적용된다(다음 단계에 있음).       
  <img src="https://user-images.githubusercontent.com/33191974/156984309-1b1a5382-b626-4530-ab91-aafe3d888779.png" width="50%" height="50%"/>  
  
설정이 완료되었으면 Continue 버튼을 클릭한다.   

> #### Apply Immediately  
> Apply Immediately를 체크해서 설정 변경 내용을 즉시 적용하면 DB 인스턴스의   
> 실행이 중지되므로 DB에 접속할 수 없게 된다.  

> #### db.m1.small  
> db.m1.small DB 인스턴스 클래스는 프리 티어에서 무료로 사용할 수 없다. 1분이라도   
> 사용하면 1시간 요금이 발생하므로 주의해야 한다.   

설정한 내용에 이상이 없는지 확인한다. 이상이 없으면 Modify DB Instance 버튼을  
클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156984930-7ff3d015-9a1d-4155-ba4c-8e3df2e85bc3.png" width="50%" height="50%"/>  
RDS DB 인스턴스의 설정이 변경중이다. 설정이 완전히 적용되기까지 약 10 ~ 15분  
정도 소요된다.     
  
RDS DB 인스턴스(exampledbinstance)의 세부 내용을 보면 DB 인스턴스 클래스와  
스토리지가 변경되었다.  

  








































