# 객체지향 설계 5원칙(SOLID)과 DI

출처: [스프링 핵심원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard),  [[개발자 면접 질문] 객체지향 프로그래밍 - 설계 5대 원칙, SOLID](https://www.youtube.com/watch?v=KO2xdqOZSAs)

## SOLID

 **로버트 마틴이 정리한 객체지향설계를 할때 지켜야하는 5가지 원칙**
 
* SRP: 단일 책임 원칙(single responsibility principle)
* OCP: 개방-폐쇄 원칙 (Open/closed principle)
* LSP: 리스코프 치환 원칙 (Liskov substitution principle)
* ISP: 인터페이스 분리 원칙 (Interface segregation principle)
* DIP: 의존관계 역전 원칙 (Dependency inversion principle)

#### 단일 책임 원칙

* 하나의 클래스는 하나의 책임만 가져야한다
  * 여기서 책임이란 기능
  * 하나의 복잡한 클래스로 정의하지말고 여러개로 쪼개서 사용해라
* 하나의 책임이라는 것의 **중요한 기준은 변경**
  * 변경했을때 파급효과가 적게 설계해야한다

#### 개방-폐쇄 원칙

* 확장에는 열려 있으나 변경에는 닫혀 있어야한다
  * 기능을 추가할때 **기존의 코드를 변경하지않고 기능추가가 가능해야함**
* **다형성**을 활용
* 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능을 구현

하지만 스프링을 사용하지 않고 개방-폐쇄 원칙을 지키기는 어렵다

```java
public class MemberServiceImpl implements MemberService{

    //private final MemberRepository memberRepository = new MemoryMemberRepository();
    private final MemberRepository memberRepository = new JdbcMemberRepository();

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```

* 다음과 같은 코드가 있을때 다형성을 활용하여 데이터타입을 MemberRepository로 선언한 경우
  * MemberRepository를 구현한 JdbcMemberRepository로 변경할때
  * 아래의 오버라이딩한 메서드들의 코드는 변경되지 않는다

**하지만**

```java
public class MemberServiceImpl implements MemberService{

    //private final MemberRepository memberRepository = new MemoryMemberRepository();
    private final MemberRepository memberRepository = new JdbcMemberRepository();

```

* 직접 사용하는 위의 코드(has a)는 변경이 되어야함 => **이러한 문제점을 스프링해서 해결하기 위해 DI컨테이너 제공**

## 리스코프 치환 원칙

* 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다
* 다형성에서 하위 클래스는 인터페이스 규약을 다 지켜야 한다는 것, 다형성을 지원하기 위한 원칙, 인터페이스를 구현한 구현체는 믿고 사용하려면, 이 원칙이 필요하다

## 인터페이스 분리 원칙

* 일반적인 하나의 인터페이스 보다는 여러개의 구체적인 인터페이스를 사용해라

## 의존관계 역전 원칙

* 의존관계를 맺을때(has a) 추상적인것(상위의 개념)에 의존해야 한다
* 즉 클라이언트가 **구현 클래스에 의존하지 말고, 인터페이스에 의존해야한다**
* 역할과 구현 중 역할에 의존해야한다

**하지만**

* MemberServiceImpl은 구현 클래스를 직접 선택한다
  * MemberRepository memberRepository = new JdbcMemberRepository();
* **DIP**에 위반

<br>

따라서 **다형성만으로 OCP, DIP를 지킬 수 없다** => 스프링의 필요성











