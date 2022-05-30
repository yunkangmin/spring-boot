RDS는 앞서 설명한 주요 기능들 이외에도 다양한 기능을 제공한다. 이 부분은 간략한  
설명으로 대신한다.  
- Parameter Groups: DB 세부 설정을 한 곳에 모아 놓은 것이다. 현재 사용하고 있는   
Parameter Group은 수정할 수 없으며, Parameter Group을 새로 생성한 뒤 DB 인스턴스  
에서 교체하는 방식으로 사용한다.   
- Option Groups: DB 실행 옵션을 한 곳에 모아 놓은 것이다. 현재 사용하고 있는   
Options Group은 수정할 수 없으며 Option Group을 새로 생성한 뒤 DB 인스턴스에서  
교체하는 방식으로 사용한다. MySQL의 경우 Memcached 설정을 할 수 있다.  
- Subnet Groups: Subnet Group에 AZ에 속한 여러 개의 Subnet을 추가할 수 있다.  
VPC를 생성하고 EC2 인스턴스와 내부 네트워크를 구성할 때 사용한다.   
- Events: DB의 인스턴스의 동작 상태, 스냅샷 생성, Read Replica 생성, 에러 발생  
등 RDS에서 발생한 모든 상황을 조회할 수 있다.  
- Event Subscriptions: DB 인스턴스의 다양한 상태를 이메일 등(SNS)을 통해 알림을  
받을 수 있다.  
   - DB 인스턴스: 백업 설정, 변경, 생성, 삭제, Failover 용량 부족, 유지관리,  
   복구 등  
   - Security Group: 설정 변경, 실패 등   
   - Parameter Group: 설정 변경  
   - Snapshot: 생성, 삭제, 실패, 복구 등  


























