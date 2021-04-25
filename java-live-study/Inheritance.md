# Week 5. 상속

- [상속 개념](#상속-개념)
- [super 키워드](#super())
- [메소드 오버라이딩](#메소드-재정의)
- [final 키워드](#final)
- [추상 클래스](#추상-클래스)
- [Object 클래스](#Object-클래스)
- [다이나믹 메소드 디스패치 (Dynamic Method Dispatch)](#Dynamic-Method-Dispatch)

<br><br>

### 상속 개념

상속은 객체 지향 프로그래밍에서 중요한 개념이다. 

상속이란 부모 클래스의 멤버(필드, 메소드)를 자식 클래스에게 물려주는 것을 뜻한다. 

<br>

현실과 다르게 프로그램에서는 자식이 부모를 선택한다(?!)

자식 클래스를 선언할 때 어떤 부모 클래스를 상속받을 것인지를 결정하고 `extends`키워드 뒤에 기술한다.

```java
class 자식클래스 extends 부모클래스{
    
}
```

중요한 건 Java는 다중상속을 허용하지 않는다. 

```java
// 다음과 같이 사용할 수 없다!
class 자식클래스 extends 부모클래스1, 부모클래스2{   
}
```

<br>

자식 클래스는 부모 클래스의 필드, 메소드, 생성자를 사용할 수 있다!

<br><br>

### super()

자식 객체를 생성하면 부모 객체가 먼저 생성되고 자식 객체가 그 다음에 생성된다. 

따라서 **자식 객체를 생성하기 위해서는 부모 객체의 생성자를 호출해야 한다!**

이게 바로 super()이다.

자식 생성자는 **첫 줄에 부모 생성자(super())를 호출**한다.

```java
public Child(){
    super();
}
```

`super`는 부모의 생성자를 호출한다.

다음과 같이 부모와 자식을 정의해보자.

#### Car.java 

```java
public class Car {

    public int maxSpeed;

    public Car(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }
}
```

#### Tesla.java

```java
public class Tesla extends Car{
    public String model;

    public Tesla() {
        //super();
    }
}
```

사실 `Tesla.java`는 컴파일 오류가 발생한다. 왜냐하면 `Car`의 기본 생성자가 없기 때문이다. 따라서 `Tesla`클래스의 기본 생성자가 `Car`의 기본 생성자를 호출할 수 없다.

따라서 `Tesla`클래스는 다음과 같이 바뀌어야 한다.

```java
public Tesla() {
    // 매개변수가 없다면 특정 상수를 이용해야 한다.
    super(100);
}

public Tesla(int maxSpeed, String model) {
    super(maxSpeed);
    this.model = model;
}
```

<br><br>

### 메소드 재정의

부모 클래스의 메소드를 자식 클래스가 사용할 수 있다. 

그런데 자식이 부모의 메소드를 바꾸고 싶을수도 있다. 

그럴때 Override를 통해 동작을 수정할 수 있다.

<br>

#### 규칙

- 부모의 메소드와 동일한 시그니처(method name, parameters)를 가져야 한다.
- 접근 제한을 더 강하게 오버라이딩할 수 없다.
- 새로운 예외(Exception)를 throws할 수 없다.

<br>

> 메소드 시그니처(Method Signature)
>
> 메소드 시그니처란 메소드의 이름과 매개변수 리스트를 뜻한다. 
>
> 같은 시그니처를 가지고 있다면 오버로딩할 수 없다.
>
> 아래의 두 메소드는 같은 시그니처를 갖고 있으므로 아래와 같이 사용할 수 없다.
>
> ```java
> void doSomething(int x){}
> int doSomething(int x){}
> ```

<br>

```java
public class Car{
    public void print(){
        System.out.println("It's car");
    }
}

public class Tesla extends Car{
    @Override
    public void print() {
        System.out.println("It's Tesla");
    }
}
```

```java
public static void main(String[] args) {
    Car car = new Car();
    Tesla tesla = new Tesla();
    
    car.print(); // It's car
    tesla.print(); // It's Tesla
}
```

<br>

#### 부모 메소드 호출(super)

기본적으로 오버라이딩하게 되면 부모 클래스의 메소드 대신 자식 메소드만 사용한다. 

만약 오버라이딩된 부모 클래스의 메소드를 사용하고 싶다면, `super`키워드를 통해 사용할 수 있다.

```java
public class Tesla{
    @Override
    public void print() {
        System.out.println("It's Tesla");
    }
    
    public void superprint(){
        super.print();
    }
}

tesla.print(); // It's Tesla
tesla.superprint(); // It's car
```

<br><br>

### final

`final`키워드는 클래스, 필드, 메소드 선언시에 사용할 수 있다. 이는 해당 선언이 최종 상태이고, 결코 수정될 수 없음을 뜻한다.

`final`키워드가 필드에 선언된다면 초기화 설정 후 더이상 값을 변경할 수 없다는 것을 뜻한다.

<br>

클래스와 메소드 선언 시에 `final`키워드는 상속과 관련이 있다.

<br>

#### final class

```java
public final class ClassName{}
```

다음과 같이 클래스에 `final`이 선언된다면 이는 **상속할 수 없는 클래스**임을 뜻한다.

즉, 다음과 같이 상속이 불가능하다.

```java
public final class String{}

public class NewString extends String{} // 불가능하다.
```

<br>

#### final method

`final` 클래스일 경우 상속이 불가능하다.

그렇다면 `final`메소드 일 경우는? 

상속과 관련이 있다고 말했다. 즉, Overriding이 불가능하다

```java
public final 리턴타입 메소드(){}
```

<br><br>

### 추상 클래스

추상(Abstract)은 실체 간에 공통되는 특성을 추출한 것을 말한다.

추상 클래스는 클래스들의 공통적인 특성을 추출해서 선언한 클래스다. 

추상클래스는 실체 클래스의 공통되는 필드와 메소드를 추출해서 만들었기 때문에 **객체를 직접 생성해서 사용할 수 없다.**

```java
public abstract class Animal{}

Animal animal = new Animal();  // 불가능
```

따라서 추상 클래스는 부모 클래스만 될 수 있으며, extends 뒤에만 올 수 있다!

```java
class Bird extends Animal{}
```

<br>

#### 용도

1. 실체 클래스들의 공통된 필드와 메소드의 이름을 통일한다.
2. 실체 클래스를 작성할 때 시간 절약한다.
   - 설계 규격을 추상 클래스를 통해 만든다면 이를 상속받아 실체 클래스 작성

<br>

#### 생성자

```java
public abstract class ClassName{}
```

추상 클래스는 `new` 연산자로 직접 생성자를 호출할 수는 없다. 하지만, 상속받아 구현한 자식 객체는 생성될 때 부모 생성자가 필요하다!

따라서 추상 클래스도 생성자가 반드시 있어야 한다.

```java
public abstract class Animal{
    public Animal(){}
}
```

<br>

#### 메서드

추상 클래스에는 일반 메소드, 추상 메소드를 사용할 수 있다. 

일반 메소드는 기존과 동일하므로 생략한다.

<br>

#### 추상 메소드

추상 메소드는 메소드의 선언만 통일화하고 실행 내용은 실체 클래스에서 구현하도록 한다. 

```java
[public | protected ] abstract 리턴타입 method(parameters);
```

다음과 같이 사용할 수 있다. 

```java
public abstract class Animal{
    public abstract void sound();
}
```

동물은 소리를 낸다는 공통적인 특징이 있으므로 `sound()` 메소드를 추상 메소드로 선언했다. 

이를 이용하여 실체 클래스를 만들어보자.

```java
public abstract class Animal{
    public String kind;
    
    public void breathe(){
        System.out.println("숨을 쉰다.");
    }
    
    public abstract void sound();
}
```

```java
public class Dog extends Animal{
    public Dog(){
        this.kind = "포유류";
    }
    
    @Override
    public void sound(){
        System.out.println("멍멍");
    }
}
```

**참고로 실체 클래스에서는 추상 메소드를 무조건 재정의해야 한다.**

<br>

### Object 클래스

`Object`클래스는 최상위 클래스다.

모든 클래스는 `Object`를 상속받는다. 

아래는 `Object`클래스의 메소드와 설명이다.

<br>

#### Override 가능 메소드

| Modifier and Type  | Method and Description                                       |
| :----------------- | :----------------------------------------------------------- |
| `protected Object` | `clone()`Creates and returns a copy of this object.          |
| `boolean`          | `equals(Object obj)`Indicates whether some other object is "equal to" this one. |
| `protected void`   | `finalize()`Called by the garbage collector on an object when garbage collection determines that there are no more references to the object. |
| `Class<?>`         | `getClass()`Returns the runtime class of this `Object`.      |
| `int`              | `hashCode()`Returns a hash code value for the object.        |
| `String`           | `toString()`Returns a string representation of the object.   |

#### Override 불가능 메소드 (final method)

| Modifier and Type | Method and Description                                       |
| :---------------- | :----------------------------------------------------------- |
| `void`            | `notify()`Wakes up a single thread that is waiting on this object's monitor. |
| `void`            | `notifyAll()`Wakes up all threads that are waiting on this object's monitor. |
| `void`            | `wait()`Causes the current thread to wait until another thread invokes the [`notify()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notify--) method or the [`notifyAll()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notifyAll--) method for this object. |
| `void`            | `wait(long timeout)`Causes the current thread to wait until either another thread invokes the [`notify()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notify--) method or the [`notifyAll()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notifyAll--) method for this object, or a specified amount of time has elapsed. |
| `void`            | `wait(long timeout, int nanos)`Causes the current thread to wait until another thread invokes the [`notify()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notify--) method or the [`notifyAll()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notifyAll--) method for this object, or some other thread interrupts the current thread, or a certain amount of real time has elapsed. |

<br>



### Dynamic Method Dispatch

Method Dispatch는 프로그램이 어떤 메소드를 호출할 지 결정하는 방법이다. 

2가지 방법이 있다.

<br>

참고로 Method Dispatch에 나온 내용과 소스코드는 모두 [토비님 유튜브](https://www.youtube.com/watch?v=s-tXAHub6vg)를 참고했다.

<br>

#### Static Method Dispatch

```java
public class Dispatch {
    static class Service{
        public void run(){
            System.out.println("run");
        }
    }

    public static void main(String[] args) {
        new Service().run();
    }
}
```

다음과 같은 경우에서는 컴파일 시점에 `Service` 클래스의 `run()`메소드를 호출할 것이라고 이미 알 수 있다. 왜냐하면 특정 클래스의 메소드를 호출했기 때문이다. 

따라서 이를 **static method dispatch**라고 한다. 

<br>

#### Dynamic Method Disptach

Dynamic Method Dispatch는 말 그대로 Dynamic한, 동적으로 호출할 Method를 결정한다.

```java
public class Dispatch {
    static abstract class Service{
        abstract void run();
    }

    static class MyService1 extends Service{

        @Override
        void run() {
            System.out.println("MyService1 run");
        }
    }
    static class MyService2 extends Service{
        @Override
        void run() {
            System.out.println("MyService2 run");
        }
    }
    
    public static void main(String[] args) {
        List<Service> svc = Arrays.asList(new MyService1(), new MyService2());

        for (Service service : svc) {
            service.run();
        }
    }
}
```

다음과 같이 실행할 때 `for`문에서 `service.run()`을 어떤 것을 실행할까? 

이는 컴파일 시점에서는 정확히 어떤 class의 메소드를 실행할 지를 알 수 없다. 

이렇게 말하면 누군가는 다 알지 않냐고 할 것이다. `MyService1`이 먼저고, `MyService2`가 두 번째다 라고 할 수 있지만, 이것은 프로그래머의 입장일 뿐 컴퓨터는 모른다. 

따라서 컴퓨터는 원소를 하나씩 꺼내보면서 해당 클래스의 `run()` 메소드를 호출한다. 

Runtime 시점에 호출할 메소드를 결정하므로 **Dynamic Method Dispatch**이다.

<br>

이 내용은 Double Dispatch와 Visitor패턴으로 발전하는데, 이는 [여기]()에 따로 정리하겠다.

<br>

<br>

### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어

[토비의 봄 TV](https://www.youtube.com/watch?v=s-tXAHub6vg)