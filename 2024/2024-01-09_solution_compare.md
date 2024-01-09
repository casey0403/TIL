# TIL

## 프롭테크
- 부동산(Property) + 기술(Technology)
- 부동산 산업 전반에 인공지능(AI), 빅데이터, 가상현실(VR) 등 첨단 정보기술(IT)을 접목한 개념
- 단순 부동산 중개 영역을 벗어나 온라인 임장, 전자계약, 인공지능 매물 추천·평가, 빅데이터 기반 자산 관리, 인테리어 서비스, 블록체인 기반 부동산 수익증권 등 서비스 영역은 무궁무진
-예시
  + 사이버 모델하우스 소개 화면
  + 인공지능 기반 매물 추천 서비스,
  + 부동산 전자계약 서비스


##  로그 수집 솔루션 비교
1. ELK(ElasticSearch LogStash Kibana) : 로그 관리툴, 레퍼런스많음/리소스 많음
2. fluentd : 무료/if문 분기 같은 상세분석 불가능
3. Datadog : 실시간/꽤 비용 존재
4. Rollbar :실시간,  server client 두 에러트레킹 모두 가능/로그 관리 별도 필요
5. Sentry  : 에러분석특화, server client 두 에러트레킹 모두 가능/로그 관리 별도 필요    
     
(각 장단점 참고) : https://velog.io/@thankspotato/%EB%8B%A4%EC%96%91%ED%95%9C-%EB%A1%9C%EA%B7%B8-%EC%88%98%EC%A7%91%EA%B8%B0-%EB%B9%84%EA%B5%90-%EB%B6%84%EC%84%9D-ELK-Fluentd-Datadog-Sentry    


## 모니터링 솔루션 비교
1. Zabbix : 오픈 소스, 쉬운 사용, all-in-one system
2. Prometheus : 오픈 소스, 빠름, easy to queyr
3. Datadog : 유료
+ SPLUNK 솔루션 : 통합 로그모니터링에 특화된 솔루션, 빅데이터 검색에 특화/licence 비쌈
- 보편적인 모니터링 사용 이유
  - 실시간 사용자, 트래픽, 트랜잭션 정보 분석
  - 서버, CPU, Memory, Disk I/O 등 서비스에 할당된 자원 현황 분석
  - 서비스 성능 개선을 위해 어떤 점을 개선해야하는지 분석 후 추천
  - 로그, 쿼리, 메소드 단위의 Deep Dive 분석
  - 마케팅 지표로 활용할 수 있는 사용자 행위 관련 모니터링  
(zabbix vs promethuues 출처)https://www.techrepublic.com/article/prometheus-vs-zabix/


## 성능 관리 서비스 비교
- APM(Application Performance Management
1. Jennifer : 유료
2. Datadog : 유료
3. Scouter : 오픈소스
4. PinPoint : 오픈소스
5. Elastic : 오픈소스
6. 국내 Whatap(:와탭) : 유료 

## 데이터 시각화 도구 
- 정보를 지도나 그래프와 같은 시각적 맥락으로 전환하는 실천
1. Grafana 오픈 소스. 데이터베이스 데이터 시각화를 위해서는 특히 모니터링과 관측에 우수한 Grafana가 높은 평가를 받고 있습니다. Grafana는 다양한 데이터 소스를 지원하며 대규모 데이터셋을 위한 강력한 시각화 기능을 제공합니다. 기업 수준의 데이터 탐색과 시각화를 위해 설계된 Apache Superset 역시 강력한 옵션 중 하나입니다.
2. PyGWalker
3. D3
4. Matplotlib
5. Plotly
5. Candela
6. Google Charts

(비교 참고) https://docs.kanaries.net/ko/articles/open-source-data-visualization-tools

## AWS 서비스  
- AWS CloudWatch  Multi AZ에서는 Prometheus 비용이 훨씬 저렴
- AWS EBS (Elastic Block Store) == 주로 HDD로 사용
- AWS S3 (Simple Storage Service) == 주로 SSD로 사용
- AWS AMI(Amazon Machine Image) == EC2 인스턴스를 실행하기 위한 정보를 모은 단위

## ISP
- Internet Service Provider, 인터넷 회사라고 일컬음.
1. 인터넷 통신망을 보유하여 인터넷 회선과 IP할당까지 담당하는 회사
  - (ex) KT, SK텔레콤, LG유플러스
2. 인터넷 통신망을 보유하고 있지만, 회선만 임대하고 IP할당은 하지 않는 회선 임대료 만으로 수익을 내는 회사
 - 한국은 대부분 1번. 거의 없다고 함.
3. 자체적으로 보유한 통신망은 없지만, 다른 회사의 통신망을 임대받고 말 그대로 인터넷 서비스만 하는 회사
  - (ex) 알뜰폰
