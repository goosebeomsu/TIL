# 스프링 컨테이너(BeanFactory, ApplicationContext)

출처: [스프링 핵심원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)

스프링은 **스프링 컨테이너를 통해 객체를 관리**하고 **스프링 컨테이너에서 관리되는 객체를 빈(Bean)** 이라고 한다

## BeanFactory와 ApplicationContext

일반적으로 BeanFactory와 ApplicationContext를 스프링컨테이너라고 부름

#### BeanFactory

* 스프링 컨테이너의 최상위 인터페이스
* 스프링 빈을 관리하고 조회하는 역할
* getBean()을 통해 조회

#### ApplicationContext

* BeanFactory의 기능을 상속받아 제공
* BeanFactory의 기능과 더불어 어플리케이션 개발에 필요한 다양한 부가기능 제공

![image](https://user-images.githubusercontent.com/83762364/184504142-47e2f61f-55df-4a60-8dc7-37faaa967512.png)


**대부분 BeanFactory대신 부가기능이 포함된 ApplicationContext를 사용**

<br>

## 스프링컨테이너 생성 및 조회

#### 생성

**1. 스프링 컨테이너 생성**

```java
   ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
```

* ApplicationContext 는 인터페이스
* 스프링컨테이너는 어노테이션 기반, XML기반 등 다양한 방식 제공
  * AnnotationConfigApplicationContext은 ApplicationContext의 구현체
  * AnnotationConfigApplicationContext로 애노테이션 기반의 자바 설정 클래스로 스프링 컨테이너를 만든다
* 생성자의 매개변수로 구성정보가 있는 클래스를 준다 (AppConfig.class)

<br>

**2. 스프링 빈 등록**

![image](https://user-images.githubusercontent.com/83762364/184504479-bbb5574c-31c4-46b6-a357-ab0a52129b9e.png)

* @Bean이 붙은 메서드를 모두 호출해서 메서드명을 빈이름(직접 부여도 가능), 리턴하는 객체를 빈객체로 스프링 빈 저장소에 저장

<br>

**3. 스프링 빈 의존관계 설정**

![image](https://user-images.githubusercontent.com/83762364/184505127-b95b68be-71cd-4d0d-8bc9-4df8bb282c00.png)

* 스프링컨테이너는 설정 정보를 참고하여 의존관계를 주입(DI)



#### 조회

```java
   MemberService memberService = ac.getBean("memberService", MemberService.class);
```

* getBean에 파라미터로 빈이름과 타입을 넣어 조회 (빈이름 혹은 타입 만으로도 조회 가능)


