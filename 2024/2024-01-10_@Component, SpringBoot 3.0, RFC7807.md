# TIL

## @Bean VS @Component 차이점
- @Bean
- @Configuration이 붙어있는 클래스를 자동으로 빈으로 등록하고 해당 클래스를 파싱해서 @Bean이 있는 메소드를 찾아서 bean을 생성
- @Bean 어노테이션은 @Configuration 어노테이션을 활용하여 Bean 등록하고자 함을 명시해주어야 한다.
- @Configuration 클래스 내에 @Bean으로 등록해야 싱글톤 보장.
- 수동으로 빈을 등록해주어야하는 상황들
   1. 개발자가 직접 제어가 불가능한 라이브러리를 활용할 때
   2. 애플리케이션 점범위적으로 사용되는 클래스를 등록할 때
   3. 다형성을 활용하여 여러 구현체를 등록해주어야 할 때
- 단점: 수동으로 빈을 등록하는 작업은 많은 시간투자 및 생산력 저하   
<br/>

- @Component
- 자동방식, 스프링도 이 방법 권장
- 특정 어노테이션이 있는 클래스를 찾아서 모두 빈으로 등록해주는 컴포넌트 스캔 기능을 제공
- 우리가 직접 개발한 클래스를 빈으로 편리하게 등록하고자 하는 경우에 활용
- @Controller, @Service, @Repository가 각 역할을 가지고있는 @Component 이다.
- @Configuration 도 @Component를 가지고 있음 (처음에 해당 클래스가 빈으로 등록되어야함   
(참고) https://wonsjung.tistory.com/396

<br/>
    
## Bean 생성 순서
1. @ComponentScan을 통해 @Component클래스의 빈 생성
- 이떄 @Configuration(@Component 어노테이션 포함되어있음) 클래스도 빈 생성된다고 생각. ~~(뇌피셜임)~~
- 단, @Configuration의 default 상태인 (proxyBeanMethods= true)인 상태에서는 @Configuration 빈은 cglib에서 생성해 준 프록시 객체이다. (@Bean 리턴 객체, 싱글톤 타입인 빈을 만들기 위해).
2. @Configuration이 붙은 클래스 내부의 @Bean이 붙은 메서드를 찾아서 메서드이름으로 된 빈을 생성한다.     
(참고)https://tecoble.techcourse.co.kr/post/2023-05-22-configuration/
- @Configuration이 사용된 클래스 내부에서는 @Bean 메소드의 내부에서 호출한 메소드가 @Bean 메소드일 경우 컨텍스트에 등록된 빈을 반환하도록 되어있음.    
(참고) https://blog.naver.com/sthwin/222131873998


## CGLIB(Code Generation Library) (cglib;)
- 코드 생성 라이브러리
- **런타임에 동적으로 자바 클래스의 프록시를 생성해주는 기능을 제공**
- 타겟에 대한 정보를 직접적으로 제공받아 바이트 코드를 조작하여 프록시를 생성
- 장점 
  + 인터페이스 없이 단순 클래스만으로 프록시 객체를 동적으로 생성 가능
  + 리플렉션이 아닌 바이트 조작을 사용, 타겟에 대한 정보를 알고 있기 때문에 JDK Dynamic Proxy에 비해 성능이 좋다.
  + (특징)구체 클래스를 상속받아 만들어졌으므로 어떤 타입으로든 타입 캐스팅 가능, 의존관계 주입도 가능
- 단점
  + 의존성 추가 (Spring 3.2 이후에는 Spring Core 패키지에 포함)
  + default 생성자 필요 (objenesis 라이브러리를 통해 해결됨)
  + 타겟의 생성자가 두번 호출(objenesis 라이브러리를 통해 해결됨)
  + final 메소드(오버라이딩 불가능), 클래스면 안됨(상속불가능). private 접근자로 된 메소드도 상속불가므로 적용안됨 프록시가 생성되지 않거나 동작하지 않음.
- CGLIB 프록시 생성과 관련된 모듈
  + Enhancer 클래스
  + Callback 인터페이스
  + CallbackFilter 인터페이스    
(참고) https://memodayoungee.tistory.com/151

## objenesis 라이브러리
- Only one purpose : To instantiate a new object of a particular class
- [생성자(인자), 다른 호출있는 생성자, 예외발생 생성자] 라고 할지라도 인스턴스를 생성해줌
- Typical uses : AOP Libraries
- How it works : Objenesis uses a variety of approaches to attempt to instantiate the object, depending on the type of object, JVM version, JVM vendor and SecurityManager present. 
- not to call any constructor or initialize any fields. To create mocks and proxies.   
(ROOT) https://objenesis.org/


## @Bean, @Component 설명
<차이점>    
1. 선언 위치 : 메소드 vs 클래스
2. 활용법 : 외부 클래스 유연하게 빈으로 등록하여 사용 vs 내 클래스를 차후 필요할 떄 스프링 사용하도록 빈으로 등록      
<br/>
  
- **@Bean**
- @Bean의 경우 개발자가 컨트롤이 불가능한 외부 라이브러리들을 Bean으로 등록하고싶을 때 사용
(ex) ObjectMapper에 @Component를 선언할 수 없으니 ObjectMapper의 인스턴스를 생성하는 메소드를 만들고 해당 메소드에 @Bean을 선언하여 Bean으로 등록
- @Target({ElementType.METHOD, ElementType.ANNOATION_TYPE})  **Method Level**    
<br/>

- **@Component**
- @Component 는 개발자가 직접 컨트롤이 가능한 Class 들의 경우에 사용
- @Target(ElementType.TYPE) TYPE으로 지정. **Class Level**   
     
- 정리 : @Bean은 사용자가 컨트롤 하지 못하는 Class나, 좀 더 유연하게 객체를 생성해서 넘기고 싶을 때, @Component는 Class 자체를 빈으로 등록하고 싶을 때, 사용하면 된다고 이해했다.   
(참고) 
- https://prodo-developer.tistory.com/107
- https://medium.com/sjk5766/bean%EA%B3%BC-component-%EC%B0%A8%EC%9D%B4-96a8d0533bfd

------ 

## Spring Context
- Spring이 Bean을 다루기 더 쉽도록 여러 기능이 추가된 공간
- Container > Context > Bean
- 종류1) Root Context (공통 부분)
  + 모든 서블릿이 공유할 수 있는 Bean들이 모인 공간
  + Repository, Service 등
- 종류2) Servlet Context (개별 부분) 
  + 서블릿 각자의 Bean들이 모인 공간. 웹 앱 마다 한개씩 존재. 웹 앱 그자체를 의미.
  + 이 Context내에서의 Bean들은 서로 공유될 수 없음
  + Controller (MVC)   
(참고) https://workshop-6349.tistory.com/entry/Spring-Spring-Context-%EC%84%A4%EB%AA%85

-------- 

## Spring Boot 3 차이점
1. java version 17 이상만
2. javax -> jakarta
3. GraalVM 
4. HTTP 에러 Response RFC 7807 지원 (ProblemDetail, ErrorResponse, ErrorResponseException)    
(참고) https://revf.tistory.com/260

## RFC 7807 
- 오류 메시지에 에러의 원인 파악하기 쉽도록.
- RFC 7807에서 제시한 오류 메시지 구조
  + type : 에러를 분류하기 위한 URI 식별자
  + title : 에러에 대한 간략한 설명
  + status : HTTP response code
  + detail : 에러에 대한 자세한 설명
  + instance : 에러 발생 근원 URI
- (ex)
  { type : "/erros/member/join/duplicate-login-name",  
    title : "DuplicatedLoginName",  
    status : 409,  
    detail : "Company member join fail due to DuplicatedLoginName",
    instance : "/api/delivery-service/company/member/join"  
  }
- 반드시 표준이 되는 것은 아니지만 참고하기 좋은 설계.
- code==status, message==title // type, detail, instance 고안해봐야할 것 같다   
(참고) https://coding-business.tistory.com/34

----- 
인용구
> “ 모니터링하지 않는 앱을 배치하는 것은 연료계 없이 자동차 여행을 떠나는 것과 같다. ”
>> - 개발자 속담(Programmer’s Proverbs)






