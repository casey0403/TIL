## 🔎Zookeeper 
- 분산 코디네이션 서비스
- use with
  + **Apache Kafka** : 서버 상태 감지를 위해 사용, 충돌 감지, 주제 검색 구현, pub-sub 상태 유지, 새로운 토픽이 생성되었을 때 토픽의 생성과 소비에 대한 상태 저장 (대규모 환경에서는 다른 서버에 두는 것을 권장. 주키퍼 앙상블은 홀수로 구성, 과반수 이상이 장애가 발생하면 중단. but 카프카는 그렇지 않아도 되기 때문에)
  + Apache HBase : Hadoop과 사용되는 데이터 저장소. 클러스터 마스터 선택 및 클러스터 메타데이터 유지하는데 사용.
  + Apache solr (;솔라) : enterprise search platform. SolrCloud 분산형태에서 클러스터에 대한 메타데이터 저장, 업데이터 조정
  + Facebook message 샤딩 및 장애 조치 구현    
(참고)https://rok93.tistory.com/entry/%EC%A3%BC%ED%82%A4%ED%8D%BCZookeeper%EB%9E%80
  
<br/>
  
-------------
## 🔎Istio
- ;이스티오, 그리스어로 돛단배
- open source, Service mesh, 애플리케이션의 서비스 간 모든 통신을 처리하는 소프트웨어 계층(어플리케이션 서비스). 가시성 & 트래픽 관리 & 보안을 담당해줄 전담 인프라 스트럭쳐 계층
  - 가시성 3대 요소(로그, 메트릭, 트레이스). 각각 다른 방법으로 수집 및 사용되지만 함께 볼 떄 인프라 및 어플리케이션에 대한 중요한 인사이트 제공  
  - 내부적으로 Envoy의 강력한 Observability 기능 덕분에 매우 쉽게 로그, 메트릭, 트레이싱을 구현
  
<br/>
     
### 로그 솔루션
  - ELK 스택이라든지
  - Sumo logic
  - Splunk
  - Amazon CloudWatch Logs       
But, MSA 환경에서 서비스끼리 어떻게 트래픽을 주고 받는지에 관한 로그는 따로 확보하기가 쉽지 않았음. 메트릭과 트레이싱도 마찬가지. 이를 Service Mesh가 가시성을 자동화된 계층으로 처리해줌.

### Service Mesh 핵심 기능
  - 1.  트래픽 관리
    + 코드의 변경 없이 서비스간의 통신을 컨트롤
    + 매우 쉬운 Canary 배포
    + 매우 쉬운 L7 라우팅 설정
  - 2. 보안
  - 3. 계층
    + 코드의 변경 없이. 그렇기 때문에 동일한 워크 로드를 Istio가 아닌 App Mesh, Consul 등과 연동해도 어플리케이션 코드에서는 변경이 필요없음.
 - 4. **Envoy**
    + 메시지를 전달해주는 사람(대리인)이라는 뜻. 실시간. API 기반 설정 업데이트 기능
  - Istio
  - AWS App Mesh
  - Hashicorp Consul
  - Kong Mesh
  - Linkerd ( Envoy 기반이 아님)
   
<br/>
    
> 가장 널리 쓰이는 Istio의 경우에도 IngressGateway, Gateway, VirtualService, DestinationRule 등 익혀야되는 개념들이 많은데 실제 상용에서 일어날 수 있는 다양한 케이스들도 무리없이 처리하려면 결국 Istio가 의존하고 있는 Envoy까지 잘 알아야 되기 때문에 Service Mesh를 잘 쓰기 위해서는 상당한 노력과 시간이 필요합니다.    
(참고) https://kr.linkedin.com/pulse/istio%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%99%9C-%EC%A4%91%EC%9A%94%ED%95%A0%EA%B9%8C-sean-lee
    
<br/>
    
------------ 
### Prometheus : monitoring
  
### **Thanos : , next level monitoring**, open source, an extension built for Prometheus(long-term storage and high availability.).
  
### Grafana : data visualization
  
### Consul : HashiCorp 개발. Service Mesh 솔루션을 제공하는 다중 네트워킹 도구
  
### Vault : HashiCorp 개발. 크로스플랫폼 패스워드 및 인증 관리 시스템/ 비공개 비밀번호, key, 토큰 저장 및 관리
  
### Ceph : 분산형 스토리지 
  
### Harbor : VMware 개발. Docker 이미지를 저장, 관리, 배포하기 위한 오픈소스 컨테이너 레지스트리




 
-------------
### 📜용어
- Canary 배포(:카나리 배포) : one of the release method. 조금씩 사용자의 범위를 늘려가며 피드백을 통해 배포하는 방식. 일부 사용자들에게 SW를 배포한 뒤 괜찮으면 사용자를 늘려가며 배포하는 방식을 뜻함. 점진적 배포라고 일컬음. 문제 발생하고 이상있는 경우 롤백.        
(참고) https://woongsin94.tistory.com/356

- 로그(Log, Logs) : 요청이 수행되는 동안 시스템의 임의의 시점에서 일어난 이벤트에 대한 기록. 보통 timestamp와 context payload를 포함.
- 메트릭(Metric Metrics) : 시간이 지남에 따라 변화하는 데이터, 메모리 사용률, CPU 사용률, 스레드 사용률 등등.. 시간에 따른 추이를 추적할 가치가 있는 데이터. 시스템의 이상 징후를 감지하고, 각기 다른 시간 간격별로 시스템의 동작을 분석하며 최근의 추세를 알아내는데 사용된다. 알림을 발생시키는 것이 효율적.
- 트레이스(Trace, Traces): 측정항목과 로그는 시스템에 대한 좋은 통찰력을 제공하지만 단일 시스템/서비스로 제한. 시스템의 다양한 구성 요소에 걸쳐 수명 주기 동안 요청의 운명을 모니터링할 수 있습니다. 각 레이어에서 수행되는 작업량을 식별하는 데 사용. Zipkin, Jaeger 인기있는 Training 오픈 소스 분산 추적 솔루션.       
(참고)
  - https://medium.com/@surfd1001/things-to-know-about-observability-mechanisms-a52876e421c7
  - (번역)https://bagbokman.tistory.com/27




