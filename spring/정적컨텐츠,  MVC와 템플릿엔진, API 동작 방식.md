# 정적컨텐츠,  MVC와 템플릿엔진, API 동작 방식

출처: [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)

스프링으로 웹 개발을 할때

* 정적 컨텐츠
* MVC와 템플릿 엔진
* API

세가지 방식을 이용

## 정적 컨텐츠

서버에서 파일을 있는 그대로 내려주는것

![image](https://user-images.githubusercontent.com/83762364/182202419-dc2ec253-223d-4306-83bc-e3efc296d839.png)

* 웹브라우저에서 hello-static.html에 접근하면 스프링 컨테이너에서 hello-static관련 컨트롤러가 있는지 확인
* 컨트롤러가 없으면 resources 밑에 static밑에 hello-static.html을 반환

<br>

## MVC와 템플릿 엔진

* MVC: 가독성, 유지보수 등을 위해 Model, View, Controller로 관심사 분리
* 템플릿 엔진: 지정된 템플릿 양식과 데이터가 합쳐져 HTML문서를 출력하는 소프트웨어, HTML을 동적으로 바꿀 수 있음 ex)타임리프, JSP

![image](https://user-images.githubusercontent.com/83762364/182203609-16741825-f863-44d7-92d5-c69cd7fb7629.png)

* hello-mvc라는 컨트롤러가 있는지 확인하고 있으면 원하는 데이터를 model에 데이터를 key-value 형식으로 담고 출력할 페이지의 확장자명을 제외한 이름을(String말고 ModelAndView return도 가능) return
* viewResolver가 templates/**hello-template**.html 을 연결시켜주고 변환 후 웹 브라우저에 출력

<br>

## API

![image](https://user-images.githubusercontent.com/83762364/182205850-3c5d18d4-1dd6-4a2a-a4df-a0c90c8a3738.png)

* **@ResponseBody 사용**
  * http body에 데이터를 넣어 리턴하겠다는 의미
  * 객체를 리턴해주면 json형식으로 데이터를 만들어서 반환(객체를 json형식으로 바꿔주는 라이브러리 jackson을 사용)
  * @ResponseBody 사용시 viewResolver를 사용하지 않고 HttpMessageConverter 사용
  * 단순문자면 StringHttpMessageConverter, 객체면 MappingJackson2HttpMessageConverter 사용
  




