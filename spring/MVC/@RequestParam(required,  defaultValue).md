# @RequestParam(required,  defaultValue)

출처: [스프링 MVC 1편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

## @RequestParam

![image](https://user-images.githubusercontent.com/83762364/189494317-46eade63-34d3-44e7-bb3c-fd2034575aa2.png)

* request.getParameter의 역할
* get방식이나 HTML Form에서 Post방식으로 온 쿼리파라미터를 조회
* @RequestParam("username")의 username은 쿼리파라미터의 key값

<br>


```java
  @RequestParam String username, @RequestParam int age
```
* **HTTP 파라미터 이름이 변수 이름과 같으면 @RequestParam(name="xx") 생략 가능**
* String , int , Integer 등의 단순 타입이면 @RequestParam 도 생략 가능하지만 가독성을 고려하자!(선택)

<br>

## requestParamRequired - 파라미터 필수 여부

![image](https://user-images.githubusercontent.com/83762364/189494822-10f92fc0-d41c-427b-99d9-f35f326b3506.png)

* required를 통해 파라미터 필수 여부를 설정 가능
* 기본값은 필수(true)
* get요청일때 /request-param-required 와 같이 요청이 들어오면 필수인 username이 없으므로 통과X

#### 주의사항

* **파라미터 이름만 사용**
  * /request-param?username= 와 같이 **파라미터 이름만 있고 값이 없으면 빈문자로 통과**
  
* **기본형(primitive)에 null 입력**
  * get으로 /request-param-required?username=park 으로 요청을 보낸다고 가정
  * Integer age가 아니라 int age라면 null을 사용 못하기 때문에 500 에러 발생(서버에러)
  * 따라서 null을 받을 수 있는 Integer나 defaultValue사용

<br>

##  requestParamDefault - 기본 값 적용

![image](https://user-images.githubusercontent.com/83762364/189495433-02c9b821-eaf9-4640-ba9c-888b67b0ea53.png)

* 파라미터에 값이 없는 경우 defaultValue를 사용하여 기본 값 적용 가능
* 이미 기본 값이 있기 때문에 required 는 의미가 없음 -> 쿼리파리미터를 아예 작성하지 않고 null이 넘어와도 기본값 적용
* defaultValue 는 빈 문자의 경우에도 설정한 기본 값이 적용




