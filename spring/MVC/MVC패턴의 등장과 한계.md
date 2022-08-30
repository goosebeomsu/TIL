# MVC패턴의 등장과 한계

출처: [스프링 MVC 1편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

## 초기의 JSP

**초기의 JSP는 HTML작성이 불편한 서블릿의 장점을 보완했지만 여전히 개선점이 존재**

![image](https://user-images.githubusercontent.com/83762364/187395319-8b3d0fb3-329e-45b0-ab2c-543f57edc2cd.png)

* 비지니스 로직과 HTML을 보여주기 위한 뷰가 한페이지에 같이 존재 => 유지보수의 어려움
* 비지니스 로직은 다른곳에서 처리하고 JSP는 View의 역할만 하기위해 MVC패턴이 필요

<br>

## MVC 패턴

#### 등장이유

* **너무 많은 역할**
  > 하나의 서블릿이나 JSP로 비지니스 로직, 뷰 렌더링을 모두 처리하게 되면 유지보수가 어려워짐
  
  > 자바코드 하나를 수정할때 수천줄의 HTML이 있는 파일에서 원하는 부분을 찾아야한다.

* **변경의 라이프 사이클**
  > UI 일부를 수정하는 일과 비지니스 로직을 수정하는 일은 다르게 발생할 가능성이 매우 높고 대부분 서로에게 영향을 주지않음
  
  > 이렇게 변경의 라이프 사이클이 다른 부분을 하나의 코드로 관리하는 것은 비효율적

* **기능 특화**
  > JSP와 같은 뷰 템플릿은 화면을 렌더링 하는데 최적화. 따라서 해당 업무만 담당하는 것이 효과적

<br>

#### 서블릿을 컨트롤러로 사용한 MVC패턴

![image](https://user-images.githubusercontent.com/83762364/187399012-e583f068-99c9-4fde-bef7-4f1818f45e8e.png)

* **컨트롤러**: HTTP요청을 받아 파라미터를 검증하고 비지니스 로직실행. 뷰에 전달할 데이터를 모델에 담아줌
* **모델**: 뷰에 출력할 데이터를 담아둠. 뷰에서 필요한 데이터를 모델에 담아서 전달해주는 덕분에 뷰는 화면 렌더링에 집중 할 수 있음
* **뷰**: 모델에 담겨있는 데이터를 사용해 화면을 그림. HTML을 생성하는 부분

**참고**

> 위와 같은 패턴에선 뷰마다 컨트롤러가 따로 존재

> 최근에는 비지니스 로직은 서비스계층에서 처리

> 모델은 문맥에 따라 여러의미가 있음 ex)비즈니스 모델이라고 할 때는 service, dao, dto등을 포함해서 설명 

<br>

## MVC패턴 한계

**MVC패턴을 통해 View는 화면을 그리는 역할에 집중하고 비지니스로직이 분리되었지만 개선점 존재**

![image](https://user-images.githubusercontent.com/83762364/187400902-658de74c-5837-4d32-bcd2-418fbdd5e877.png)

#### 중복코드 발생

```java
  RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
  dispatcher.forward(request, response);
```

* View로 이동하는 코드가 항상 중복 호출되어야 한다. 물론 이 부분을 메서드로 공통화해도 되지만, 해당
메서드도 항상 직접 호출해야 한다

<br>

```java
  String viewPath = "/WEB-INF/views/members.jsp";
```

* /WEB-INF/views/, .jsp 가 중복. 이때 jsp가 아닌 타임리프로 변경한다면 전체코드를 다 변경해야함

<br>

#### 사용하지 않는 코드

```java
  HttpServletRequest request, HttpServletResponse response
```

* 위와 같은 코드들은 사용될때도 있고 안될때도 있음

#### 공통처리 어려움

* 기능이 복잡해질때 컨트롤러에서 공통으로 처리해야 하는 부분이 증가
* 이때 공통처리를 하려면 각각의 컨트롤러에서 공통로직을 다 적용해야함

<br>

**이러한 문제점을 해결하기 위해 수문장 역할을 하는 프론트 컨트롤러(하나의 입구) 패턴을 도입!**








