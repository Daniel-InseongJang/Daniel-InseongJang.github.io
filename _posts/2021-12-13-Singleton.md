---
title: "[JAVA] Singleton"
tags: JAVA DesignPattern CreationalPattern
article_header:
  type: cover
  image:
    src: /screenshot.jpg
---

# Singleton 
---	
## 싱글턴 패턴
인스턴스를 오직 한개만 제공하는 클래스 이며, 생성패턴중(Creational Design Pattern) 하나이다.<br>
생성 패턴은 객체 생성 메커니즘을 다루는 디자인 패턴으로 시스템을 객체가 생성, 구성 및 표현되는 방식과 분리하는 것을 목표로 한다.<br>


두가 지 목적
- 해당 클래스의 인스턴스를 오직 하나만 만들수 있어야한다.
- 만들어진 하나의 인스턴스의 글로벌에게 접근할수있는 방법을 제공해야 한다.<br> 
시스템 런타임, 환경 세팅에 대한 정보, 인스턴스가 여러개 일 때 문제가 생길수 있는 경우가 있다.<br>
  인스턴스를 오직 한개만 만들어 제공하는 클래스가 필요하다.


```java
public class LazyInitialized {

    private static LazyInitialized lazyInitialized;
    private LazyInitialized() {}

    public static LazyInitialized getLazyInitialized() {
        if (lazyInitialized == null) {
            lazyInitialized = new LazyInitialized();
        }
        return lazyInitialized;
    }
}
```
이 코드의 문제가 하나있다.
우리가 application을 만들때 멀티스레드를 사용하게 된다.<br>
그럴경우 이코드는 안전하지않다. 왜냐하면 if의 intance가 null 인지 체크하고 null일경우 <br>
새로운 인스턴스를 만드는데 만들기전 다른스레드에서 접근해서 새로운 인스턴스를 만들어 버릴수있다.<br> 
이러면 하나의 인스턴스만 제공해야하는 싱글턴이 깨지게 된다. 그리고 private 생성자는 상속이 불가능하다.

## Multi-Thread 에서 안전한 방법 
### 1.synchronized 사용하기
```java
public class ThreadSafeInstance {

    private static ThreadSafeInstance threadSafeInstance;
    private ThreadSafeInstance() {}

    public static synchronized ThreadSafeInstance getThreadSafeInstance() {
        if (threadSafeInstance == null) {
            threadSafeInstance = new ThreadSafeInstance();
        }
        return threadSafeInstance;
    }
}
```
이 메소드에 한번에 딱 하나의 스레드만 들어오게 하는 방법이다.<br>
이렇게하면 멀티스레드에서도 안전하다.<br>
단점은 호출 할때 마다 동기화 처리 작업 때문에 성능의 불이득이 생길수 있다.

### 2.eager initialization(이른 초기화)

```java
public class EagerInitialized {

    private static final EagerInitialized EagerInitialized = new EagerInitialized();
    private EagerInitialized() {}

    public static EagerInitialized getEagerInitialized() {
        return EagerInitialized;
    }
}
```
이 객체를 나중에 안만들어도 되고 이객체를 만드는 비용이 크지않을경우 미리 만들수 있다.<br>
이 방법도 멀티스레드에 안전하며, 이 인스턴스는 클래가 로딩되는시점에 그때 static한 필드들이 초기화 된다.<br>
단점은 미리만드는게 단점이 될수도있다. <br>
이 인스턴스를 만드는과정에서 오래걸리고 메모리를 많이 사용한다면 로딩할때 많은 리소스를 썻음에도 불구하고 안쓰는 경우가 된다.<br>  

### 3.double checked locking 기법 (volatile 를 왜써야하는지)
```java
public class Settings {

    private static volatile Settings instance;
    private Settings() {}

    public static Settings getInstance() {
        if (instance == null) {
            synchronized (Settings.class) {
                if (instance == null) {
                    instance = new Settings();
                }
            }
        }
        return instance;
    }
}
```
1번째 스레드가 if 문통과하고 두번째 스레드가 if문을 통과했다 하더라도 먼저 도착한 1번째 스레드가<br> 
instance를 만들때까지 기다려야한다. 그리고 두번째 스레드는 if문을 확인후 이미 만들어져있으므로<br>
멀티스레드에 안전해진다.<br>

### 4.static inner class 사용 (권장!)
```java
public class BillPughSingleton {

    private BillPughSingleton() {}

    private static class BillPughSingletonHolder {
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }


    public static BillPughSingleton getInstance() {
        return BillPughSingletonHolder.INSTANCE;
    }
}
```
Bill Pugh 가 제안한 방벙이다. 이렇게 하면 멀티스레드에도 안전하고 
getInstance() 호출될때 SettingsHolder 클래스가 로딩이 되고
그때 INSTANCE를 만들므로 Lazy loading도 가능한 코드가 된다.

### 5.Enum 사용 (권장!)
```java
public enum EnumSingleton {

    INSTANCE;
    
    public static void do(){
    }
}
```
Reflection을 해결하기 위한 방법 으로 Joshua Bloch는 Singleton 디자인 패턴을 구현하기 위해 Enum을 사용할 것을 제안했다. Java 프로그램에서 모든 enum 값이 한 번만 인스턴스화 되도록 보장한다. 그리고 직렬화&역직렬화에 안전한방법이다. 왜냐하면 기본적으로 Serializable를 구현하고 있다.  단점은 enum은 다소 융통성이 없다는 것입니다. 예를 들어 지연 초기화(lazy initialization)를 허용하지 않는다.


## 싱글턴 깨기!

1. 리플렉션 사용하기
- 셋팅스 안에 컨스트럭터를 사용해 만들면 된다.
- 대응방법 이뉴멀레이션을 사용하면 된다.(이늄은 리플렉션에서 생성을 못하게 막아놨다)
- 이늄의 단점은 미리만들어진다. 클래스를 로딩하는순간 이미 만들어진다. 그리고 이늄은 이늄만 상속이가능해 상속불가하다.
2. 직렬화 & 역직렬화 사용하기
- 역직렬화를 할때는 무조건 생성자를 생성해서 인스턴스를 만들어야 하므로 새로운 인스턴스가 된다.
- 대응방법은 readResolve라는 메소드를 정의하면 이걸 사용하게 된다.



**참고**

[Journaldev](https://www.journaldev.com/1377/java-singleton-design-pattern-best-practices-examples)<br>
[코딩으로 학습하는 GoF의 디자인 패턴](https://www.inflearn.com/course/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)
