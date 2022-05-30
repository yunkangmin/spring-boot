ElastiCache Redis 클러스터와 캐시 노드가 완전히 생성되었더라도 엔드포인트 주소로  
접속이 되지 않는다. Redis 클러스터를 생성할 때 Security Group을 기본값인  
default(VPC)로 설정했다. 이 default(VPC)는 모든 트래픽에 대해 Inbound가 열려   
있지만 접속 가능한 IP 대역(Source)은 default 자기 자신으로 설정되어 있다.   
즉 같은 default(VPC) Security Group 설정 안에서만 접속이 허용되므로 외부에서는  
접속할 수 없다. 따라서 Redis 클러스터 전용 Security Group을 생성하고 포트(6379)를  
열어줘야 한다.   
  
RDS와 ElastiCache는 큰 차이점이 있다. RDS의 데이터베이스 엔진은 AWS 외부(인터넷)  
접속이 허용되어 있지만 ElastiCache의 캐시 엔진은 AWS 외부에서 접속할 수 없다.  
Security Group을 생성하여 모든 IP 대역에 대해 접속을 허용하더라도 동일한 VPC에  
속한 EC2 인스턴스에서만 접속할 수 있다.   
  
Security Group 생성과 ElastiCache Redis 클러스터 보안그룹 변경, telnet 접속은   
[ElastiCach Memcached 클러스터 Security Group 생성 및 설정하기](https://github.com/yunkangmin/spring-boot/blob/main/aws/80%20ElastiCach%20Memcached%20%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%20Security%20Group%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.md)  
를 참고하자. 보안그룹에서 포트만 6379로 변경하거나 추가한다.   
  
telnet 접속 시에는 아래를 참고한다.  
```
[ec2-user@ip-172-31-23-183 ~]$ telnet exampleredis-001.jyqn3e.0001.apn2.cache.amazonaws.com 6379
Trying 172.31.1.236...
Connected to exampleredis-001.jyqn3e.0001.apn2.cache.amazonaws.com.
Escape character is '^]'.
info
$4257
# Server
redis_version:6.2.5
redis_git_sha1:0
redis_git_dirty:0
redis_build_id:0
redis_mode:standalone
os:Amazon ElastiCache
arch_bits:64
multiplexing_api:epoll
atomicvar_api:c11-builtin
gcc_version:0.0.0
process_id:1
run_id:133863b990a0f0e29472afde545febd775fd624f
tcp_port:6379
server_time_usec:1646750098433922
uptime_in_seconds:1922
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:2582930
executable:-
config_file:-

# Clients
connected_clients:3
cluster_connections:0
maxclients:20000
client_recent_max_input_buffer:32
client_recent_max_output_buffer:0
blocked_clients:0
tracking_clients:0
clients_in_timeout_table:0
...
```

> #### redis-cli   
> Redis는 rediscli라는 전용 클라이언트를 제공하고 있다. 다운로드 및 설치 방법은  
> 다음 링크를 참조하기 바란다.  
> http://redis.io/download




























