---
title: "[Java] 정적 팩토리 메소드(Static Factory Method)"
tags: Java
article_header:
  type: cover
  image:
    src: /screenshot.jpg
---

정적 팩토리 메서드(Static Factory Method)?
===

클래스내에서 객체생성하는 메소드를 말한다.<br/>
우리가 평소에쓰는 new 를 써서 새로운 생성자를 만드는게 아니라 메소드를 써서<br/>
객체를 생성하는것을 의미합니다.<br/>
팩토리 메서드 패턴과는 다른 패턴이다. 이름만 비슷하다.<br/>


***그럼 메소드를 써서 생성자를 만드는 이유가 뭘까?***<br/>
- 목적의 맞는 이름을 가질수있어서 new로 생성자를 만드는것보다 명시적으로
  표현할수있어 소스를 이해하고 사용하기 편하다.
- 호출시 매번 새로운 생성자를 만들필요없다
- 상속을 사용할때 하위 자료형 객체를 반환할수있다.
- 객체생성시 캡슐화하여 내부상태를 외부로 드러낼 필요가없어 단순화시킬수있다.
- 형인자 자료형(parameterized type) 객체를 만들 때 편하다.<br/>


**단점**


- 상속을 위해서는 public 혹은 protected 생성자가 필요
- 정적 팩토리 메서드만 제공하면 하위 클래스를 만들 수 없다.
- 즉, 앞서 예시로 든 Collections 객체의 하위 클래스를 생성할 수 없다.
- 개발자가 정적 팩토리 메서드를 찾기 어렵다.
- 이 문제를 보완하려면 정적 팩토리 메서드 이름을 잘 지어야한다.
  ex) getInstance() : 누가봐도 인스턴스를 반환하는 정적 팩토리 메서드지 않은가?<br/>


**Naming conventions**
- from : 하나의 매개 변수를 받아서 객체를 생성
- of : 여러개의 매개 변수를 받아서 객체를 생성
- getInstance , instance : 인스턴스를 생성. 이전에 반환했던 것과 같을 수 있음.
- newInstance , create : 새로운 인스턴스를 생성
- get[OtherType] : 다른 타입의 인스턴스를 생성. 이전에 반환했던 것과 같을 수 있음.
- new[OtherType] : 다른 타입의 새로운 인스턴스를 생성.<br/>

Lombok의 RequiredArgsConstructor를 사용하면 정적 팩토리 메소드를 쉽게 만들 수 있다.<br/>
~~~java
@RequiredArgsConstructor(staticName = "of")
public class User {
 private final Long id;
 private final String name;
}
~~~
~~~java
User user = User.of(1L, "Man");
~~~