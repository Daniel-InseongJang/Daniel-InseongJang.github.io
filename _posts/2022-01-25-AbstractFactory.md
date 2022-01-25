---
title: "[JAVA] AbstractFactory"
tags: JAVA DesignPattern
article_header:
  type: cover
  image:
    src: /screenshot.jpg
---

# AbstractFactory
## 추상 팩토리 (Abstract Factory) 패턴
관련있는 여러 객체를 만들어주는 인터페이스.<br>
구체적인 팩토리에서 인스턴스를 만드는것 까지는 팩토리 메소드 패턴과 비슷하지만 초점이 클라이언트 쪽에 있다.<br> 
팩토리를 사용하는 클라이언트 초점에 맞춰서 바라봐야 한다. 팩토리를 사용하는쪽 코드와 같이 봐야 한다.

### 팩토리 메소드 와 추상 팩토리의 차이점 
1. 팩토리 메소드 패턴 은 객체를 만드는 과정에 좀더 집중되어 있다.
2. 팩토리 메소드 패턴 과 추상 팩토리 패턴 의 같은점은 객체를 만드는 관점을 추상화 하는것은 맞다.
3. 추상 팩토리 패턴은 클라이언트 관점에서 보면 팩토리를 통해서 추상화된 인터페이스만 클라이언트가 사용 할수 있게 해주기 때문에 클라이언트 입장에서 ConcreteClass를 직접 참조해서 사용할 필요가 없어진다.

--- 
## 예제 코드 
### 필요한 기본 코드

```java
public class Car {
    private String name; //이름
    private String vehicle; //차 종류
    private String color; // 색
    private Tire tire; // 타이어
    private Wheel wheel; // 휠
}
```

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

### Before
지금 이 클래스가 위에 설명했던 클라이언트 코드의 해당한다.
```java
public class BusFactory implements CarFactory {

    @Override
    public Car createCar() {
        Car car = new Bus();
        car.setTire(new WhiteTire());
        car.setWheel(new WhiteWheel());
        return car;
    }
}
```
비슷한 제품에 다른 WhiteNewTire, WhiteNewWheel 로 변경 하려면 해당 코드를 변경 해야 하고<br>
다른 종류의 제품이 추가 되면 코드를 변경 해줘야 한다.<br>
이 코드가 변경되지 않게 하면서도 다른 종류의 제품을 추가하는 방법을 추상 팩토리 패턴을 이용하면 된다.<br>
간단히 말해 추상팩토리 패턴을 적용해 WhiteNewTire, WhiteNewWheel 같은 구체적인 클래스의 의존하지 않게 한다.<br>

```java
public class WhiteTire implements Tire {
}
```
```java
public class WhiteWheel implements Wheel {
}
```
### After
우선 추상 팩토리를 만든다.
```java
public interface CarPartsFactory {

    Tire createTire();

    Wheel createWheel();
}
```
추상 팩토리에 구체적인 팩토리를 만든다.<br>
만약 다른 종류의 Tire 와 Wheel을 원하면 이 추상 팩토리를 이용해 구현하면된다. 
```java
public class BusPartsFactory implements CarPartsFactory {

    @Override
    public Tire createTire() {
        return new WhiteTire();
    }

    @Override
    public Wheel createWheel() {
        return new WhiteWheel();
    }
}
```

팩토리를 사용하기위해 추상팩토리를 사용한다.<br>
팩토리 타입의 인스턴스를 받아 인스턴스 받은 팩토리를 통해서 Tire 와 Wheel을 가져오면<br>
해당 코드는 새로운 종류의 Tire 와 Wheel를 추가하더라도 변경되지 않을 것이다. 
```java
public class BusFactory implements CarFactory {

    private CarPartsFactory carPartsFactory;

    public BusFactory(CarPartsFactory carPartsFactory) {
        this.carPartsFactory = carPartsFactory;
    }

    @Override
    public Car createCar() {
        Car car = new Bus();
        car.setTire(carPartsFactory.createTire());
        car.setWheel(carPartsFactory.createWheel());
        return car;
    }
}
```

결과<br>
추상 팩토리를 구현한 BusPartsFactory 사용해 원하는 종류의 Tire 와 Wheel을 사용할수 있다. 
```java
public class CarInventory {

    public static void main(String[] args) {
        CarFactory carFactory = new BusFactory(new BusPartsFactory());
        Car car = carFactory.createCar();
        System.out.println(car.getTire().getClass());
        System.out.println(car.getWheel().getClass());
    }
}
```

결과값<br>
class _03_abstractFactory._1_before.WhiteTire<br>
class _03_abstractFactory._1_before.WhiteWheel<br>

**참고**
[코딩으로 학습하는 GoF의 디자인 패턴](https://www.inflearn.com/course/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)

