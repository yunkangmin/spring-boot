ElasticCache에서는 EC2나 RDS와는 달리 인스턴스를 캐시 노드라고 부른다.  
이 캐시노드는 EC2, RDS처럼 여러 사양으로 나뉘어져 있다.   
  
ElasticCache의 캐시 노드가 여러가 사양으로 나뉘어져 있는 이유는 비용 절감과   
효율성때문이다. 사용량이 적으면 낮은 사양의 인스턴스를 사용하면 되고, 사용량이  
많아 부하가 크면 높은 사양을 사용하면 된다. 이처럼 사용자에게 선택권을 준다는  
것이 큰 장점이다.   
  
단, RDS와의 차이점은 RDS는 생성된 DB 인스턴스의 인스턴스 클래스를 변경할 수   
있지만, ElasticCache는 생성된 캐시 노드의 캐시 노드 유형을 변경할 수 없다.  
한 번 생성하면 캐시 노드 유형은 고정되며 캐시 노드 유형을 바꾸려면 삭제한 후   
다시 생성해야 한다(2014년 8월 기준).  
  
ElasticCache 캐시 노드는 cache.m1.small처럼 cache로 시작하며 인스턴스 패밀리인  
m에 세대(Generation)를 뜻하는 숫자가 붙고 .(점)뒤는 전체적인 사양 규모를 뜻하는  
단어가 붙는다.   
  
캐시 노드 유형은 다음과 같다.   
- 마이크로: 가격이 가장 싼 캐시 노드이다. 낮은 vCPU 성능과 213MB 메모리를   
제공한다. 프리티어에서는 이 인스턴스 유형을 무료로 사용할 수 있다. cache.t1.  
micro가 있다.   
- 표준: vCPU, 메모리, 네트워크, I/O 용량 등이 평균적인 사양으로 제공된다.    
cache.m1.small, cache.m1.medium, cache.m1.large, cache.m1.xlarge등이 있다.  
- 고급형: 표준보다 vCPU 개수와 메모리 용량이 더 많다. cache.m3.xlarge, cache.  
m3.2xlarge등이 있다. 단, 이 유형은 Memcached만 사용할 수 있다.   
- 고용량 메모리: 고급형보다는 vCPU 개수가 작지만 메모리 용량이 훨씬 크다. 또한,  
높은 I/O 용량을 제공한다. cache.m2.xlarge, cache.m2.xlarge, cache.m2.4xlarge  
등이 있다.  
- 고성능 CPU: 다른 캐시 노드 유형보다 vCPU 개수와 성능이 높다. cache.c1.xlarge등이  
있다.   

ElastiCache 캐시 노드의 기본 구매 옵션은 온 디맨드 캐시 노드이며 사용한 시간  
만큼 요금을 지불하는 방식이다. 자세한 요금은 AWS 사이트의 요금표([http://aws.amazon.com/ko/elasticache/pricing/](http://aws.amazon.com/ko/elasticache/pricing/)를  
참조하기 바란다.   
  
ElastiCache 캐시 노드는 EC2 인스턴스와는 달리 SSH로 접속할 수 없다.  
  
> #### Amazon ElasticCache 캐시 노드 유형   
> [http://aws.amazon.com/ko/elasticache/details/](http://aws.amazon.com/ko/elasticache/details/)































