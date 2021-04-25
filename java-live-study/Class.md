# Week 4. 클래스

<br>

### 객체 지향 프로그래밍(Object-Oriented Programming)

흔히 Java는 객체 지향 언어라고 한다. 그러면 객체 지향은 무엇일까?

<br>

#### 객체

객체(Object)란 물리적으로 존재하거나 추상적으로 생각할 수 있는 것 중에서 자신의 속성을 가지고 다른 것과 식별 가능한 것을 말한다.

객체는 속성과 동작으로 구성되어 있다.

- 속성 : 객체의 특징을 뜻한다. 필드라고 한다.
- 동작 : 객체의 역할, 책임을 뜻한다. 메서드라고 한다.

현실 세계의 문제를 소프트웨어 세계의 객체로 프로그래밍하는 것이 객체 지향 프로그래밍이다. (현실 세계의 객체와 SW 세계의 객체는 엄연히 다르게 봐야한다!)

<br>

#### 상호작용

객체는 서로 상호작용한다. 

`사람`이란 객체는 `계산기`라는 객체의 메소드를 호출하여 계산을 할 수 있다. 

이처럼 객체가 다른 객체의 기능을 이용하는 것이 메소드 호출이다. 

<br>

### OOP 특징

객체지향하면 생각나는 특징이 있다. 

특징을 간단히 살펴보자.

<br>

#### 캡슐화(Encapsulation)

캡슐화란 객체의 필드, 메소드를 하나로 묶고, 실제 구현 내용을 감추는 것을 말한다. 

따라서 외부 객체는 객체의 내부를 알지 못하며, 노출된 필드, 메소드만 이용할 수 있다.

<br>

#### 상속(Inheritance)

일상에서 흔히 사용하는 상속의 뜻이 맞다. 

부모 역할의 상위 객체와 자식 역할의 하위 객체가 있다. 

하위 객체는 상위 객체의 필드, 메소드를 물려받아 사용할 수 있다. 

<br>

**상속**은 

- 상위 객체를 재사용해서 하위 객체를 쉽고 빨리 설계할 수 있도록 도와준다. 
- 재사용을 통해 반복된 코드의 중복을 줄여준다

<br>

#### 다형성(Polymorphism)

다형성은 많을 다, 모양 형을 사용한다. 

이름처럼 같은 타입이지만 실행 결과가 다양한 객체를 이용할 수 있는 성질을 말한다. 

??? 말이 어렵다. 좀 더 공부하다보면 이해가 갈 것이다.

간단히 설명하자면, 부모 객체 타입에는 모든 자식 객체가, 인터페이스 타입에는 모든 구현 객체가 대입될 수 있다. 

<br>

다형성을 통해 **객체는 부품화가 가능하다.** 

<br>

### 클래스

내가 테슬라 모델 S를 가지고 있다고 해보자. 

그런데 친구가 똑같은 모델 S를 샀다! 그러면 친구의 차와 나의 차는 동일한 차인가? 

차의 종류는 같지만, 엄연히 다른 차이다. 번호판도 다를테고 지금까지 주행한 거리나, 타이어 상태도 다를 수 있다.

그런데 생각해보면 테슬라는 분명 어떤 설계도를 가지고 자동차를 만들 것이다. 

출고가 되면서 다른 차가 되지만 기본적으로 가진 스펙은 같을 것이다. 

<br>

그런데 자동차도 객체라고 보면 객체 지향 프로그래밍에서도 적용할 수 있다. 

사용할 객체마다 설계해서 각자 다른 설계를 가지게 한다면 비용이 너무 많이 든다. 

따라서 객체도 공통된 설계도를 통해 객체를 생성할 수 있도록 한다. 

<br>

**설계도 = 클래스** 이다. (다만 착각하지 마라, 객체 지향 프로그래밍은 단어 그대로 '객체' 지향이다. '클래스' 지향이 아니다!)

<br>

그럼 이제 클래스를 어떻게 설계하고 사용하는지 알아보자. 

<br>

### 클래스 선언

사용할 객체를 구상했다면, 이제 대표 이름 하나를 결정하고 클래스 이름으로 정한다. 

클래스 이름은 다른 클래스와 식별할 목적으로 사용되므로 규칙에 따라서 만들어야 한다!

| 번호 | 작성 규칙                                                    | 예              |
| ---- | ------------------------------------------------------------ | --------------- |
| 1    | 하나 이상의 문자로 이루어진다.                               | Car, SportsCar  |
| 2    | 첫 번째 글자는 숫자가 올 수 없다.                            | Car, 3Car(X)    |
| 3    | '$', '_'외의 특수 문자는 사용할 수 없다.                     | $Car, @Car(X)   |
| 4    | Java 키워드는 사용할 수 없다.                                | int(X)          |
| -    | (관례) 클래스 첫 자는 대문자, 단어 혼합일경우 각 단어의 첫 자를 대문자 | Car, ChatServer |

클래스 선언은 중괄호를 통해 선언한다.

```java
public class 클래스이름{
    
}

class 클래스이름2{
    
}
```

<br>

### 객체 생성과 클래스 변수

객체 생성은 new 키워드를 통해 할 수 있다. 

다음 코드를 작성하게 되면 메모리 힙 영역에는 객체가 생성된다.

```java
클래스 변수;
변수 = new 클래스();

Car car;
car = new Car();

클래스 변수 = new 클래스();

Car car = new Car();
```

<br>

### 클래스 구성

클래스의 구성에는 필드(Field), 생성자(Constructor), 메소드(Method)가 있다. 생략되거나 복수 개가 작성될 수 있다.

```java
public class ClassName{
    int fieldName;
    
    ClassName(){}
    
    void methodName(){}
}
```

<br>

### 필드

필드는 객체의 데이터, 참조 객체, 정보를 저장한다. 따라서 필드는 **객체에 소속된 데이터**다

선언 형태는 변수와 비슷하지만, 필드를 변수라고 부르지 않는다. 

> 클래스 내 변수
>
> 변수는 생성자와 메소드 내에서만 사용되고 소멸된다!

[Week2](https://github.com/gashe-soo/java-study/blob/master/java-live-study/week2.md#%EB%B3%80%EC%88%98%EC%9D%98-%EC%8A%A4%EC%BD%94%ED%94%84%EC%99%80-%EB%9D%BC%EC%9D%B4%ED%94%84%ED%83%80%EC%9E%84)의 변수 라이프 스코프를 참고하자.

<br>

필드는 객체에 소속되어 있기 때문에 사용하기 위해서는 객체가 생성되어 있어야 한다.

객체가 존재하지 않으면 필드도 존재하지 않는다. 

```java
public class Person{
    int age;
    
    Person(){
		age = 10;
    }
}
// 불가
Person.age = 15;

// 가능
Person person = new Person();
person.age = 15;
```

<br>

그런데 여기서 얘기하는 건 인스턴스 필드이다. => 객체에 소속된 필드이다. 

<br>



### 생성자

생성자는 `new`연산자로 호출되는 특별한 중괄호 블록이다. 

생성자가 성공적으로 실행되면 힙 영역에 객체가 생성되고 객체의 주소가 리턴된다.

객체 생성 시 초기화를 담당한다. 리턴 타입은 없다.

<br>

#### 기본 생성자 

```java
public class Class{
    
}
```

기본 생성자는 생성자를 선언하지 않아도 컴파일 과정에 추가된다.

그러나 **다른 생성자를 명시적으로 선언한다면, 컴파일러는 기본 생성자를 추가하지 않는다.**

<br>

#### 생성자 선언

```java
클래스(매개변수선언,...){
    //객체 초기화 코드
}
```

다음과 같은 클래스가 있다고 가정하자.

```java
public class Car {

    String company = "Tesla";
    int maxSpeed;
    int drivingDistance;
}
```

현재는 명시적인 생성자가 없으므로 기본 생성자는 생성된다.

어떤 결과가 나올까?

```java
Car car = new Car();
System.out.println(car.company);
```

```
Tesla
```

가 나온다. 

이제 생성자를 추가해보자.

```java
public class Car {

    String company = "Tesla";
    int maxSpeed;
    int drivingDistance;

    public Car(String name, int speed, int distance) {
        company = name;
        maxSpeed = speed;
        drivingDistance = distance;
    }
}
```

다음 결과는 어떻게 나올까?

```java
Car car = new Car("Hyundai", 100, 0);
System.out.println(car.company);
```

```
Hyundai
```

필드를 초기화해도 생성자에서 초기화하는 것이 반영된다!

<br>

#### 생성자 오버로딩

여러 생성자를 만들 수 있다. 오버로딩을 통해 매개변수에 따른 생성자를 만들 수 있다!

```java
public class Car {

    String company = "Tesla";
    String model;
    String color;
    int maxSpeed;

    public Car() {
    }

    public Car(String model) {
        this.model = model;
    }

    public Car(String model, String color) {
        this.model = model;
        this.color = color;
    }

    public Car(String model, String color, int maxSpeed) {
        this.model = model;
        this.color = color;
        this.maxSpeed = maxSpeed;
    }
}
```

<br>

그런데 오버로딩이 많아질 경우 문제가 발생할 수 있다.

매개 변수의 수만 다르고 필드 초기화 내용이 비슷하다면 중복된 코드가 발생한다. 

이럴 때는 `this()`를 사용한다. 이는 자신의 다른 생성자를 호출하는 코드다. 반드시 생성자의 첫 줄에서만 허용된다!

<br>

같은 필드 초기화 내용으로 인해 중복 코드가 발생했다.

```java
public Car(String model) {
    this.model = model;
    this.color = "red";
    this.maxSpeed = 200;        
}

public Car(String model, String color) {
    this.model = model;
    this.color = color;
    this.maxSpeed = 200;
}

public Car(String model, String color, int maxSpeed) {
    this.model = model;
    this.color = color;
    this.maxSpeed = maxSpeed;
}
```

<br>

```java
public Car(String model) {
    this(model, "red", 200);
}

public Car(String model, String color) {
    this(model, color, 200);
}

public Car(String model, String color, int maxSpeed) {
    this.model = model;
    this.color = color;
    this.maxSpeed = maxSpeed;
}
```

<br>

### 메소드 

객체의 동작에 해당하는 중괄호 블록이다. 

메소드는 매우 중요하다. 다른 객체와 통신하며 데이터를 주고 받는 수단이기 때문이다!

<br>

```java
리턴타입 메소드이름(매개변수...){
    //실행할 코드
}
```

메소드 이름도 규칙이 있다.

- 숫자로 시작하면 안되고 $,_를 제외한 특수 문자를 사용하면 안된다.
- 관례적으로 메소드명은 소문자로 시작한다.
- 단어가 혼합된다면 뒤이어 오는 단어의 첫머리 글자는 대문자로 작성한다.

```java
void run(){}
String getName(){}
int[] getScores(){}
```

<br>

#### 매개변수 수가 변하는 가변 인자

```java
// 매개변수를 배열 타입으로 선언한다.다만 넘겨주기 전에 배열을 생성해야 한다.
int sum(int[] values){}

// 매개변수를 ...을 이용해 선언. 이는 자동으로 배열을 생성된다.
int sum(int... values){}
```

<br>

#### 메소드 오버로딩

메소드 오버로딩의 조건은 매개 변수의 타입, 개수, 순서 중 하나가 달라야 한다.

```java
public double sum(int x, double y){
    return x+y;
}

public double sum(double x, int y){
    return x+y;
}
```

이렇게 순서만 바뀌어도 가능하다!

<br>

### this 키워드

객체 내부에서 인스턴스 멤버에 접근하기 위해 `this`를 사용한다. 

사실 우리는 위에서 잠깐 봤다.

```java
public Car(String model, String color, int maxSpeed) {
    this.model = model;
    this.color = color;
    this.maxSpeed = maxSpeed;
}
```

매개변수에 같은 이름의 변수가 있어 this를 통해 객체 자신의 변수를 명시할 수 있다.

<br>

이뿐만 아니라 객체 자체를 return할때도 사용된다.

```java
public class Car{
    
    public Car getInstance(){
        return this;
    }
}
```



<br>

### 정적 필드, 메서드

#### 정적 필드(static)

정적 필드는 클래스에 고정된 멤버로서 객체를 생성하지 않고 사용할 수 있다. 

```java
public class Class{
    static 타입 필드;
}
```

정적 필드는 클래스로더가 클래스를 로딩할 때 메소드 메모리 영역에 적재한다. 따라서 로딩이 끝나면 객체 생성 없이 사용할 수 있다.

<br>

예를 들어 `계산기`클래스의 파이(pi)는 변하지 않고 공용 데이터이므로 정적 필드로 선언할 수 있다.

```java
public class Calculator{
    static double pi = 3.14159;
}
```

정적 필드는 다음과 같이 사용할 수 있다.

```java
System.out.println(Calculator.pi);
```

<br>

#### 정적 메소드

정적 메소드 또한 필드처럼 클래스 생성과 상관없이 로딩만 되면 사용할 수 있다.

```java
public class Calculator{
    static double pi = 3.14159;
    static int plus(int x, int y){
        return x + y;
    }
}

int result = Calculator.plus(5,4);
```

<br>

그런데 사실 객체를 생성해도 사용은 가능하다.

```java
Calculator testCalculator = new Calculator();
int result = testCalculator.plus(5,4);
```

<br>

#### 정적 초기화 블록

기존에는 선언과 동시에 초기값을 주었다.

```java
static double pi = 3.14159;
```

그러나 계산이 필요한 초기화 작업이 있을 수도 있다. 

이럴 때는 어떻게 해야 할까? 

정적 필드는 생성자에서 초기화할 수 없으므로 정적 블록을 통해 초기화한다.

```java
static double pi;

static{
    pi = 3.14159;
}
```

그런데 이렇게 쓰면 어떻게 될까?

```java
public class Car {

    static String company;

    static{
        company = "Tesla";
    }
    
    public Car() {
        company = "Hyundai";
    }
}

System.out.println(Car.company);
Car testCar = new Car();
System.out.println(testCar.company);
```

결과는 다음과 같다

```
Tesla
Hyundai
```

정적 필드가 객체가 생성되면서 생성자에서 다른 값을 할당하면서 값이 바뀌게 되었다.

<br>

### 접근 제한자

객체는 외부 클래스에서 접근할 수 있는 멤버와 접근할 수 없는 멤버로 구분해서 설계하는 것이 바람직하다. 

왜냐하면 누가 맘대로 내 정보를 바꿀 수 있으니 이를 막아야 하기 때문이다. 

따라서 이를 위해 접근 제한자가 존재한다. 

<br>

| 접근 제한 | 적용 대상                         | 같은 클래스 | 같은 패키지 | 자손 클래스 | 모두 |
| --------- | --------------------------------- | ----------- | ----------- | ----------- | ---- |
| public    | Class, Field, Constructor, Method | O           | O           | O           | O    |
| protected | Field, Constructor, Method        | O           | O           | O           | X    |
| default   | Class, Field, Constructor, Method | O           | O           | X           | X    |
| private   | Field, Constructor, Method        | O           | X           | X           | X    |

<br><br>



### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어



