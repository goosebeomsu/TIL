# MVC프레임워크 만들기

출처: [스프링 MVC 1편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

**프론트 컨트롤러 패턴을 도입해 DispatcherServlet과 유사한 MVC프레임워크를 점진적으로 구현해 보자**

<br>

![image](https://user-images.githubusercontent.com/83762364/188283555-3675e4bb-42d8-4b66-9850-24737f4c40b1.png)

**FrontController 패턴 특징**
* 프론트 컨트롤러가 서블릿 하나로 클라이언트 요청을 받음
* 프론트 컨트롤러가 요청에 맞는 컨트롤러를 찾아 호출
* 입구를 하나로 처리
* 공통 처리 가능
* 프론트 컨트롤러를 제외한 나머지 컨트롤러는 서블릿을 사용하지 않아도 됨

## 프론트 컨트롤러 도입 - v1

![image](https://user-images.githubusercontent.com/83762364/188283668-16aab55f-1b3b-4860-bb95-2a625e8d3fd3.png)

<br>

![image](https://user-images.githubusercontent.com/83762364/188284111-e154d363-fc16-462f-8239-b90f623a3a19.png)

**urlPatterns**
* /front-controller/v1을 포함한 하위 모든 요청은 위의 프론트 컨트롤러로 들어옴

**controllerMap**
* key: 매핑 URL
* value: 호출될 컨트롤러

**service**
* requestURI를 조회해서 실제 호출할 컨트롤러를 호출하고 없다면 404 상태코드 반환
* 컨트롤러를 찾고 controller.process(request, response); 을 호출해서 해당 컨트롤러를 실행
  > 인터페이스로 만든 컨트롤러를 구현한 각각의 컨트롤러를 다형성을 통해 사용

## View 분리 - v2



```java
String viewPath = "/WEB-INF/views/new-form.jsp";
RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
dispatcher.forward(request, response);
```

* 실제 사용하는 구현된 컨트롤러에서는 뷰로 이동하는 부분에 위와 같은 중복이 발생
* 이를 처리하기 위해 별도로 뷰를 처리하는 객체를 생성

<br>

![image](https://user-images.githubusercontent.com/83762364/188284462-fcd8dab1-9197-4b35-8638-5f153014cc53.png)

<br>

#### MyView 객체

```java
public class MyView {

  private String viewPath;
  
  public MyView(String viewPath) {
    this.viewPath = viewPath;
  }
  
  public void render(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
    dispatcher.forward(request, response);
 }
}
```

* 매개변수가 있는 생성자를 통해 생성하면서 viewPath값을 세팅
* render 메서드를 통해 JSP실행

#### 실제 사용하는 컨트롤러

```java
  return new MyView("/WEB-INF/views/members.jsp");
```

* 구현한 컨트롤러에서 viewPath를 파라미터로 넘겨 생성한 MyView객체를 리턴

#### 프론트 컨트롤러


```java
  MyView view = controller.process(request, response);
  view.render(request, response);
```

* 프론트 컨트롤러에서 컨트롤러의 process메서드를 실행하여 MyView객체를 리턴받음
* MyView의 render 메서드를 통해 JSP실행

<br>

**이전 코드**
![image](https://user-images.githubusercontent.com/83762364/188285038-d8db92fe-f6fb-4d34-8ece-5e63b50b0694.png)


**개선 코드**
![image](https://user-images.githubusercontent.com/83762364/188285098-f3773cf1-fa3e-4f21-a85a-9b14ed71054c.png)

* 우리가 구현한 컨트롤러에서 **View와 관련된 코드의 중복 개선**


















