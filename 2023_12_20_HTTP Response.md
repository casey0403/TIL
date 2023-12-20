# 공통 응답 포맷을 위한 공부

## HTTP (HyperText Transfer Protocol)
요청과 응답을 처리하기 위한 규약

* HTTP Request
  - Start Line : Method, URL, HTTP version
  - Headers : OS, Broswer, 인증정보 etc...
  - Body : 응답 정보 (ex). Json, Html....

* HTTP Response
  - Status Line : HTTP version, 처리 상태 (ex) 200, 400
  - Headers 
  - Body : 

## 1. @ResponseBody 어노테이션
- Spring에서는 @ResponseBody 어노테이션을 활용하여 쉽게 HTTP 요청에 대한 응답으로 응답 객체를 직렬화하여 Body에 만들어준다. 
- HttpMessageConverter 활용
- But, 
단점1. Header에 대한 유연한 설정 힘듬
단점2. Status 설정은 따로 해주어야함 (따로 @ResponseStatus 활용하거나 해야함)

## 2. ResponseEntity 객체
ResponseEntity extends HttpEntity
Object status;

HttpEntity
HttpHeaders headers;
T body;

빌더 사용 권장
return new ResponseEntity.ok().body(response);


https://tecoble.techcourse.co.kr/post/2021-05-10-response-entity/







