---
title: "[JAVA] Factory Method"
tags: JAVA DesignPattern
article_header:
  type: cover
  image:
    src: /screenshot.jpg
---

# Factory Method
## 팩토리 메드(Factory method) 패턴
인스턴스를 생성하는 책임을 구체적인 클래스(ConcreateClass) 가 아니라 추상적인 인터페이스를 통해 서브 클래스가 정한다. 
---
## 장점 과 단점 
### 장점
- 기존 인스턴스의 생성 코드를 건드리지 않고 같은 부류의 새로운 인스턴스를 다른방법으로 얼마든지 확장가능하게 해준다.
- 확장에 열려있고 변경에 닫혀있다.(기존코드를 변경하지 않교 새로운기능을 얼마든지 확장할수있게 만드는 원칙)
- 기존코드를 건드리지 않기 때문에 코드가 더 간결해지고 기존코드가 복잡해지지 않는다. 
### 단점
- 클래스가 늘어나는 단점. 

--- 
## 예제 코드 
### Before
```java
public class Car {
    private String name; //이름
    private String vehicle; //차 종류
    private String color; // 색
}
```

```java
public class BeforeFactoryMethodTest {
    public static void main(String[] args) {
        Car bus = CarFactory.order("bus", "BUS", "white");
        System.out.println(bus);

        Car ambulance = CarFactory.order("AMBULANCE", "AMBULANCE", "white");
        System.out.println(ambulance);
    }
}
```

```java
public class CarFactory {

    public static Car order(String name, String vehicle, String color) {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException();
        }

        if (vehicle == null || vehicle.isBlank()) {
            throw new IllegalArgumentException();
        }

        if (color == null || color.isBlank()) {
            throw new IllegalArgumentException();
        }
        System.out.println("만들기전");

        Car car = new Car();
        car.setName(name);
        car.setVehicle(vehicle);
        car.setColor(color);

        System.out.println(vehicle + " 생성 완료");

        return car;
    }

}
```

```
결과 값

만들기전
BUS 생성 완료

만들기전
AMBULANCE 생성 완료
```

이 코드의 단점은 새로운 공정을 추가한다고할때 코드의 중복이 늘어나고 기존 코드를 수정해야할게 늘어난다 이렇게 했을 경우 가독성도 떨어지게 된다.

### After

```java
public class Car {
    private String name; //이름
    private String vehicle; //차 종류
    private String color; // 색
}
```

Car를 상속받은 Bus 와 Ambulance 를 만든다.

```java
public class Bus extends Car {
    public Bus() {
        setName("bus");
        setVehicle("BUS");
        setColor("white");
    }
}
```

```java
public class Ambulance extends Car {
    public Ambulance() {
        setName("Ambulance");
        setVehicle("AMBULANCE");
        setColor("white");
    }
}
```

CarFactory를 추상화 한다.

```java
public interface CarFactory {

    default Car order(String name, String vehicle, String color) {
        validate(name, vehicle, color);
        beforeMessage();
        Car car = createCar(name, vehicle, color);
        afterMessage(vehicle);
        return car;
    }

    Car createCar(String name, String vehicle, String color);

    private void validate(String name, String vehicle, String color) {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException();
        }

        if (vehicle == null || vehicle.isBlank()) {
            throw new IllegalArgumentException();
        }

        if (color == null || color.isBlank()) {
            throw new IllegalArgumentException();
        }
    }

    private void beforeMessage() {
        System.out.println("만들기전");
    }

    private void afterMessage(String vehicle) {
        System.out.println(vehicle + " 생성 완료");
    }
}
```

```java
public class BusFactory implements CarFactory {

    @Override
    public Car createCar(String name, String vehicle, String color) {
        Car car = new Car();
        car.setName("bus");
        car.setVehicle("BUS");
        car.setColor("white");
        return car;
    }
}
```

```java
public class AmbulanceFactory implements CarFactory {

    @Override
    public Car createCar(String name, String vehicle, String color) {
        Car car = new Car();
        car.setName("Ambulance");
        car.setVehicle("AMBULANCE");
        car.setColor("white");
        return car;
    }
}
```

결과
```java
public class BeforeFactoryMethodTest {
    public static void main(String[] args) {
        Car bus = new BusFactory().order("bus", "BUS", "white");
        System.out.println(bus);

        Car ambulance = new AmbulanceFactory().order("AMBULANCE", "AMBULANCE", "white");
        System.out.println(ambulance);
    }
}
```

```
결과 값

만들기전
BUS 생성 완료

만들기전
AMBULANCE 생성 완료
```

이렇게 변경 했을때 새로운 제품을 만드는 공정을 추가한다고 하면 기존 코드는 변경되지 않는다.<br>
이렇게 해서 개방-폐쇄 원칙(OCP, Open-Closed Principle) 을 지킬수 있다.

---

## 팩토리 메소드 패턴 찾기
- Java 의 Calender
- Spring 의 BeanFactory

**참고**
[코딩으로 학습하는 GoF의 디자인 패턴](https://www.inflearn.com/course/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)
