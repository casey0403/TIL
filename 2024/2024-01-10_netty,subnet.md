# TIL


## Netty    
- 네트워크 개발자가 쉽게 개발할 수 있도록 이벤트를 논리적으로 구분하여 고수준의 추상화 이벤트 모델을 제공
- 채팅형 시스템 비롯한 이벤트 기반 애플리케이션, 대규모 분산 시스템,  클라우드 애플리케이션에 꾸준히 사용, 
- WebSocket, TCP 사용하면서 주로 사용
- 3가지 구조
  + 응용프로그램 Layer : 실제 비즈니스 로직
  + 전송 Layer : Low level의 네트워크 프로토콜을 추상화한 후 API를 제공하여 단순하게 송신. 데이터를 송수신하는 API 마련
  + 네트워크 Layer : 세부정보를 파악하여 입출력 관리. 연결 여부 확인. Eventloop를 확인하여 관리

- 각각 모듈화되어 있어서 상호호환
  + 응용프로그램 Layer :  받아올 데이터에 대한 가공 및 보안 처리
  + 전송 Layer : 어떻게 데이터를 담을 것인지 판단하고 데이터의 모습을 그림
  + 네크워크 Layer : 연결에 대한 부분을 담당


[과거] 기존의 소켓 통신 프로그래밍(Socket API)은 동기적인 네트워크 통신
  - 클라이언트:스레드=1:1
  - 리소스 낭비, 무한 대기(좀비), 오버헤드 현상

[Java] NIO(Non-Blocking Input Ouput)는 비동기적인 네트워크 통신
  - java.nio.channels.Selector (1Thread:1Selector:NChannel) : selector에서 필요할 때마다 스레드에게 통지
  - 언제든지 읽기나 쓰기 작업의 완료 상태를 확인할 수 있으므로 한 스레드로 여러 동시 연결을 처리할 수 있다
  - 입출력을 처리하지 않을 때는 스레드를 다른 작업에 활용할 수 있다.
  - 순수 자바 라이브러리를 이용해서 커넥션을 통한 네트워크를 구축하는 것은 매우 어려움.

[Netty] Netty 프레임워크
- 내부 로직이 이미 구현
- NIO 클라이언트 서버 프레임 워크.  Java 기반의 비동기식 이벤트 기반 네트워크 프레임워크
- 프로토콜 서버 및 클라이언트와 같은 네트워크 응용 프로그램을 빠르고 쉽게 개발 가능
- Tomcat서버가 10,000건의 커넥션을 처리한다면, netty는 100,000건에서 1,000,000건의 커넥션을 처리할 수 있는 장점이 있다.
- 핵심 컴포넌트 Channel, CallBack, Future, 이벤트와 핸들러, Event Loop, PipeLine 등
  + Channel :  하나 이상의 입출력 작업을 수행할 수 있는 하드웨어 장치, 파일, 네트워크 소켓, 프로그램 컴포넌트와 같은 엔티티에 대한 열린 연결. 인바운드, 아웃바운드를 위한 운송수단.
  + Callback : 관심 대상에게 작업 완료를 알리는 일반적인 방법. 네티는 내부 콜백 이용. 
  + Future : 작업이 완료되면 어플리케이션에 이를 알리는 방법(비동기 결과 알림). Java Netty는 ChannelFuture 사용. operationComplete() 콜백 메소드 호출. 비동기 이벤트 기반
  + 이벤트와 핸들러 : 작업 상태 변화를 알리기 위한 고유한 이벤트 사용. 로깅, 데이터 변환, 흐름 제어, 어플리케이션 논리 등의 동작. 각 핸들러 인스턴스는 특정 이벤트에 반응하여 실행하는 일종의 콜백이라고 이해.
  + Event Loop : Event Loop Group:Event Loop = 1:N.  Channel : Event Loop = N:1 (한 Channel은 수명주기 동안 Event Loop에 등록. 한 Event Loop는 하나 이상의 Channel로 할당 가능)
  + Channel Pipeline : 

(참고)  
https://stylishc.tistory.com/151 

 
## Socket
- Socket 통신 : 단방향 통신
- HTTP 통신 : 양방향 통신
- 네트워크에서의 소켓은 프로그램을 의미, 멀리 떨어져있는 host 사이에 데이터를 주고 받을 수 있도록 프로그램으로 구현한 것
- 소켓 종류
  + 스트림 : TCP 사용. 양방향. 연결을 맺는 행위 있음(오버헤드 있을 수 있음). 대용량 가능. 
  + 데이터그램 : UDP 사용. 명시적으로 연결을 맺지 않으므로 비연결형소켓이라고 함. 스트림보다 신뢰성 낮음.
  + RAW : TCP,UDP계층을 우회하여 바로 어플리케이션으로 송신하는 소켓. 원형 패킷 볼 수 있음. 거의 드물게 사용(시스템 소프트웨어, 패킷 분석 프로그램 개발)


## 서브넷
- 서브네트워크(네트워크 내부의 네크워크)
- 네트워크를 보다 효율적으로 만들어줌
- 서브넷을 통해 네트워크 트래픽은 불필요한 라우터를 통과하지 않고 거리를 이동하여 대상에 도달.

- IP주소란?
  + 인터넷에 연결되는 모든 장치에는 고유한 인터넷 프로토콜(IP) 주소가 할당.
  + IPv4 _._._._ == 네트워크 클래스 + 장치 위치

- 서브넷은 IP 주소를 장치 범위 내에서 사용하도록 좁혀 준다. 
- 서브넷 마스크란?
  + 서브넷 마스크는 IP 주소와 비슷하지만 네트워크 내에서 내부적으로만 사용한다.
  + 네트워크 내의 라우터는 서브넷 마스크를 사용하여 데이터를 하위 네트워크로 정렬. 즉 데이터 패킷을 올바른 위치로 라우팅한다. 
  + 서브넷 마스크는 인터넷을 통과하는 데이터 패킷 내에서 표시되지 않으며, 이러한 패킷은 라우터가 서브넷과 일치하는 대상 IP 주소만 나타낸다.
  + 서브넷 마스크가 없으면 IP 네트워크 장치의 정확한 위치를 찾기 위한 시간이 더 소모된다.

(편지 예시1) 
- A 사람이 B 사람의 집이 아닌 회사로 편지를 보냄
- B의 회사로 전달 -> B가 속한 팀으로 전달 -> B에게 편지 전달
- B==IP주소, 속한팀==서브넷마스크
(실제 예시2) 
- IP주소 192.0.2.15 / 클래스 C 네트워크라면 네트워크는 192.0.2(192.0.2.0/24)로 식별. 네트워크 라우터는 패킷을 192.0.2 라고 표시된 네트워크의 호스트로 전달
- 해당 네트워크에 ㅗ착하면 네트워크 내의 라우터가 라우팅 테이블을 참조. 서브넷 스크 255.255.255.0을 사용하여 이진법 계산. 장치 주소 15를 확인하고 패킷이 이동해야 서브넷을 계산.
- 해당 서브넷 내에서 패킷을 전달하는 라우터 또는 스위치로 전달. 패킷은 IP주소 192.0.2.15에 도착.


### 용어
- CSRF Cross Site Request Forgery 사이트 간 요청 위조     
- Eureka server 클라이언트-API서버 중간에 있는 미들웨어 서버. (관련 용어) Spring Cloud, MSA, Spring Eureka
