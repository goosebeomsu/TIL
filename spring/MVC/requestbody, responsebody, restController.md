# requestbody, responsebody, restController

출처: [스프링 MVC 1편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

<br>

Get 이나 HTML Form의 Post방식으로 요청해서 View의 이름을 리턴하는 방식이 아니라

JSON, TEXT 와 같은 데이터들을 **HTTP message body**에 직접 담아서 요청하고 응답할때 requestbody, responsebody 사용



![image](https://user-images.githubusercontent.com/83762364/189495832-258aa56b-05fc-4f6b-a85a-5a535d98ee3a.png)

## @RequestBody

* HTTP 메시지 컨버터가 HTTP message body의 내용을 String으로 변환하거나 JSON의 경우 객체로 변환
* HTTP 요청시에 content-type이 application/json 이여만 JSON을 처리할 수 있는 HTTP 메시지 컨버터가 실행

<br>

## @ResponseBody

* 응답할때 View의 이름을 리턴해서 뷰 템플릿을 사용하지않고 직접 HTTP메세지 바디에 응답결과를 담아 전달
* 객체의 경우 HTTP 메시지 컨버터가 객체를 JSON으로 변환해서 응답

<br>

## @ResponseStatus & ResponseEntity

#### ResponseEntity

![image](https://user-images.githubusercontent.com/83762364/189497415-48ef2106-e57e-46c0-ba8c-496f9eb3ef20.png)

* ResponseEntity는 HttpEntity를 상속받음
* HttpEntity는 요청과 응답에 필요한 HTTP 메시지의 헤더, 바디정보를 가지고 있음
* ResponseEntity 는 여기에 더해서 HTTP 응답 코드를 설정 가능

#### @ResponseStatus

![image](https://user-images.githubusercontent.com/83762364/189497565-c82e1ca5-bd93-419f-8e10-295ab00ba547.png)

* ResponseEntity 는 HTTP 응답 코드를 설정할 수 있지만 @ResponseBody를 사용하면 설정하기 까다로움
* @ResponseStatus 애노테이션을 통해 응답 코드 설정 가능
* 하지만 애노테이션이기 때문에 응답코드를 동적으로 변경 불가

**조건에 따라서 동적으로 HTTP 상태코드를 변경하려면 ResponseEntity 사용**

<br>

## @RestController

@Controller 대신에 @RestController 애노테이션을 사용하면, **해당 컨트롤러의 메서드 전체에 @ResponseBody 가 적용되는 효과**

뷰 템플릿을 사용하는 것이 아니라, **HTTP 메시지 바디에 직접 데이터를 입력하는 방식인 Rest API를 만들때 사용하는 컨트롤러**










