---
title: "[JAVA] Prototype"
tags: JAVA DesignPattern
article_header:
  type: cover
  image:
    src: /screenshot.jpg
---

# Prototype
## 프로토타입 (Prototype) 패턴
기존 인스턴스를 복제하여 새로운 인스턴스를 만드는 방법, 기존 객체를 응용해서 새로운 인스턴스를 만들때 사용.<br>
데이터 베이스에서 가져온 데이터로 인스턴스를 만들경우 오래걸리는데 <br>
매번 이런식으로 생성하면 이 인스턴스를 만들때마다 리소스를 많이 사용하므로 한번 가져온 인스턴스를 <br>
복사를해서 새로운 인스턴스를 만들고 필요한값만 변경해서 쓰는 방법이다.<br>


## 잠점과 단점
### 장점
1. 복잡한 객체를 만드는 과정을 숨긴다.
2. 기존 객체를 복제하는 과정이 새 인스턴스를 만드는 것보다 효율적일수 있다.
3. 추상적인 타입을 리턴할수 있다. 

### 단점
1. 복제한 객체를 만드는 과정에 제약이 많아질수록 복잡해 진다.(순환 참조가 있는 경우)

--- 
## 예제코드
[Prototype 패턴 예제 참조](https://www.journaldev.com/1440/prototype-design-pattern-in-java)

**참고**<br>
[코딩으로 학습하는 GoF의 디자인 패턴](https://www.inflearn.com/course/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)

