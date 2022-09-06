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


## Model 추가 - v3

**서블릿 종속성 제거**
* 구현한 컨트롤러가  HttpServletRequest, HttpServletResponse 을 사용하지 않도록 바꿈
* request.getParameter를 통해 파라미터 정보를 찾지 않고 Map을 통해 파라미터 정보를 넘기는 방식으로 변경
* request.setAttribute로 데이터를 View에 넘기지 않고 별도의 Model 객체를 만들어 반환

**뷰 이름 중복 제거**
* /WEB-INF/views/new-form.jsp -> **new-form**
* 위와 같이 뷰의 논리 이름을 반환하면 실제 물리적인 이름은 프론트 컨트롤러에서 처리하도록 변경
* 이렇게 하면 뷰의 폴더 위치가 변경되어도 프론트컨트롤러만 고치면 됨

![image](https://user-images.githubusercontent.com/83762364/188301037-8e625361-8904-46a9-acca-c9dc8801446c.png)

#### ModelView 객체

```java
public class ModelView {
    private String viewName;
    private Map<String, Object> model = new HashMap<>();

    public ModelView(String viewName) {
        this.viewName = viewName;
    }
}
```

* 구현한 컨트롤러에서 ModelView를 리턴
* viewName에 뷰의 논리이름, model에 뷰로 전달한 데이터를 담음


#### 프론트 컨트롤러

**V2 코드**

```java
 MyView view = controller.process(request, response);
 view.render(request, response);
```

**V3 코드**

```java
 Map<String, String> paramMap = createParamMap(request);

 ModelView mv = controller.process(paramMap);
 String viewName = mv.getViewName();

 MyView view = viewResolver(viewName);

 view.render(mv.getModel(), request, response);
```

* 프론트 컨트롤러에서 클라이언트로 부터 온 파라미터 정보를 맵에 담는다
* 구현한 컨트롤러에 매개변수로 request와 response대신 paramMap을 넘긴다
* 컨트롤러에서는 paramMap을 통해 파라미터를 조회하고 View의 논리이름과 View로 전달할 데이터의 정보가 담겨있는 ModelView를 리턴
* 프론트 컨트롤러에서 모델뷰에 담겨 있는 논리이름을 조회하여 viewResolver를 통해 실제 물리이름으로 변경하여 실제 경로가 담겨있는 MyView를 리턴 
* 기존의 render에서 model을 추가로 받는 메서드를 통해 request.setAttribute를 render에서 처리

<br>

**이전 코드**

![image](https://user-images.githubusercontent.com/83762364/188576294-7da01f58-11ca-4dd1-8849-b6315a0a1bd3.png)

**개선 코드**

![image](https://user-images.githubusercontent.com/83762364/188576444-bc7a7f92-ed1a-4212-aaca-81356756bf1f.png)

## 단순하고 실용적인 컨트롤러 - v4

**ModelView대신 String 리턴**

* 개발자가 실제 구현한 컨트롤러에서 항상 ModelView 객체를 생성하여 반환해야해서 번거로움
* 컨트롤러에서 ModelView대신 String 형식의 ViewName만 리턴해서 번거로움을 줄임

![image](https://user-images.githubusercontent.com/83762364/188578149-53e2d47e-14b5-4f9a-912d-ef4311ec134c.png)

#### 프론트 컨트롤러

**V3 코드**

```java
        Map<String, String> paramMap = createParamMap(request);

        ModelView mv = controller.process(paramMap);
        String viewName = mv.getViewName();

        MyView view = viewResolver(viewName);

        view.render(mv.getModel(), request, response);
```

**V4 코드**

```java
        Map<String, String> paramMap = createParamMap(request);
        Map<String, Object> model = new HashMap<>();

        String viewName = controller.process(paramMap, model);

        MyView view = viewResolver(viewName);
        view.render(model, request, response);
```

* model객체를 프론트컨트롤러에서 미리 생성한다
* controller의 process메서드에 파라미터로 model을 같이 넘기고 viewName을 리턴
* 구현한 컨트롤러에서는 ModelView를 사용하지 않고 파라미터로 넘어온 model에 데이터를 넣고 viewName을 리턴

<br>

**이전 코드**

![image](https://user-images.githubusercontent.com/83762364/188581287-b11eb2f8-480f-470e-8362-f5435b6b2d4e.png)

**개선 코드**

![image](https://user-images.githubusercontent.com/83762364/188581447-a18ea991-168c-49e6-8523-7e15a5de77da.png)


## 유연한 컨트롤러 - v5

**어댑터 패턴 적용**

* 어떤 개발자는 ModelView를 리턴하고 싶고 어떤 개발자는 viewName을 리턴하고 싶을때
* 어댑터 패턴을 통해 프론트 컨트롤러가 다양한 방식의 컨트롤러를 처리할 수 있도록 변경

![image](https://user-images.githubusercontent.com/83762364/188582448-cefdddde-9798-4ed2-aba3-f20c00fbc226.png)

프론트 컨트롤러는 핸들러(컨트롤러) 매핑 정보에서 요청 URL를 기반으로 핸들러를 조회한다. 다음으로 조회한 핸들러를 지원할 수 있는 핸들러 어댑터를 핸들러 어댑터 목록에서 찾는다. 이제 프론트 컨트롤러는 컨트롤러를 직접 호출하지 않고, 핸들러 어댑터가 대신 호출해준다. 프론트 컨트롤러는 핸들러 어댑터가 반환해준 넘어온 ModelView 객체를 가지고 논리 이름을 물리 이름으로 바꾸어 렌더링해주면 된다.

#### MyHandlerAdapter

![image](https://user-images.githubusercontent.com/83762364/188586130-9826e0de-487e-4ac1-96cd-6131d6e4e9d7.png)

* **supports**: 어댑터가 해당 컨트롤러를 처리할 수 있으면 true반환
* **handle**: 컨트롤러를 호출하고 ModelView반환, ModelView를 반환하지 않는 경우, 어댑터가 직접 생성

#### ControllerV3HandlerAdapter

![image](https://user-images.githubusercontent.com/83762364/188587243-9b7e9a3d-1ba9-45c1-afda-50c5da6464d9.png)

* ModelView를 리턴하는 ControllerV3를 지원하는 핸들러 어댑터
* supports를 통해 매개변수로 들어오는 핸들러가 처리할 수 있는 핸들러인지 검사
* 컨트롤러의 process를 호출하여 ModelView를 프론트 컨트롤러에 전달

#### ControllerV4HandlerAdapter

![image](https://user-images.githubusercontent.com/83762364/188588326-f524cc41-b600-43f9-8433-fe4e7013ee4c.png)

* 컨트롤러가 viewName을 리턴하는 경우 어댑터에서 ModelView를 생성해서 리턴

#### FrontControllerV5 - 핸들러 매핑 정보, 핸들러 어댑터 목록

![image](https://user-images.githubusercontent.com/83762364/188588872-9aaf35f8-b85c-41af-a195-6773c16e600f.png)

* initHandlerMappingMap을 통해 사용할 컨트롤러들을 추가
* initHandlerAdapters를 통해 사용할 어댑터들을 추가
* FrontController는 생성될 때 handlerMappingMap과 handlerAdapters를 가지고 있다

#### FrontControllerV5 - 메서드

![image](https://user-images.githubusercontent.com/83762364/188590721-b08daf80-f4da-4e8f-b8af-e44fadcf001c.png)

* **getHandler**: 요청URI를 통해 handlerMappingMap에 있는 사용할 컨트롤러를 찾음
* **getHandlerAdapter**: MyHandlerAdapter에서 컨트롤러에 맞는 어댑터를 찾아서 리턴































