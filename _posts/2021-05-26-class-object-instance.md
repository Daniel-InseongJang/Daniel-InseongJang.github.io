---
title: "[Java] Class vs Object vs Instance"
tags: Java
article_header:
  type: cover
  image:
    src: /screenshot.jpg
---

**class vs object vs instance**

class 를 설계도 라고 한다.

object 를 소프트웨어에서 규현할 대상 이라고 한다.

왜 클래스를 설계도라고 할까? object를 소프트웨어 세계에 구현할 대상 이라고 할까?<br>
인스턴스는 class 와 object 차이를 알면 될거같은데...<br>
이차이를 나는 완벽하게 인지 하지 못하고있다. <br>
이번 목표에서 이걸 최대한 이해하고 설명해 보랴고 한다.

자동차 로 예를 들어보면 차에는 종류(type:suv,sedan,coupe,truck), 색(color), 브랜드(brand) 등이 있을수 있다.<br>
자동차는 달릴수도(run) 있고 , 멈출수도(stop) 있다.

위에 예제에서 단순하게 생각해보면<br>
car - 클래스 <br>

type<br>
color<br>
brand<br>
는 Date members 또는 objec 이며

run<br>
stop<br>
은 method 이다.



class vs object difference<br>
- class 는 소프트웨어세서 객체를 생성하기위한 설계도이다.
- object 는 class의 인스턴스 이다.
- class 는 논리적 entity 이고 객체는 물리적 entity 이다.
- class 는 메모리 공간응 할당받지 않고 object는 할당 받는디ㅏ.

Instance
oop의 관점에서 객체가 메모리에 할당되어 실제 사용될 때 ‘인스턴스’라고 부른다.

<!--more-->
