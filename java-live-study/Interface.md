# Interface

- [인터페이스란?](#인터페이스란?)
- [인터페이스 정의](#인터페이스-정의)
- [인터페이스 구현](#인터페이스-구현)
- [익명 구현 객체](#익명-구현-객체)
- [인터페이스 상속](#인터페이스-상속)
- [Default Method](#Default-Method)
- [Static Method](#Static-Method)
- [Private Method](#Private-Method)
- [Functional Interface](#Functional-Interface)
- [추상클래스와 인터페이스](#추상클래스와-인터페이스)

<br><br>

### 인터페이스란?

인터페이스는 객체의 사용 방법을 정의한 타입이다. 

인터페이스는 객체간 서로 통신하는 접점 역할을 한다.

<br>

#### 사용 이유

객체는 다른 객체의 메소드를 직접 호출하면 간단한데, 왜 인터페이스를 두어야 할까?

답은 **코드를 수정하지 않고, 사용하는 객체를 변경할 수 있도록 하기 위해서**이다. 

예를 통해 이해해보자. 

> 회사원 A는 아침마다 커피가 필요하다. 그래서 매일 커피를 사러 간다. 
>
> 카페를 가서 점원에게 커피를 시켰다. 주문받은 점원은 커피를 만들어서 A에게 준다. 
>
> 이것을 객체간의 통신으로 본다면 회사원 A는 커피를 반환하는 점원의 메소드를 호출한 것이다. 
>
> 그렇다면 점원은 커피를 반환하기만 하면 된다. 또한, 회사원 A는 커피를 받기만 하면 된다. 
>
> 점원이 커피를 어떻게 만드는 지는 회사원 A의 관심  밖이다. 
>
> 이해가 되는가? 안된다면 좀 더 들여다보자.
>
> 카페에서 점원 B와 점원 C가 격일로 일한다. 
>
> 오늘은 점원 B가 일하는 날이다. 회사원 A는 오늘도 커피를 주문했다.
>
> 점원 B는 자신만의 루틴이 있다. 커피를 만들기 전에 담을 컵을 씻고 준비해둔다. 그리고 커피를 만든다.
>
> 결과는 회사원 A는 커피를 잘 받았다. 
>
> 그리고 하루가 지났다. 오늘은 점원 C가 일하는 날이다. 회사원 A는 어김없이 커피를 주문하러 왔다.
>
> 점원 C는 점원 B와 다르게 커피를 만들기 전에 커피를 내리면서 컵을 준비한다. 
>
> 결과는 회사원 A는 커피를 잘 받았다. 
>
>
> 이처럼 회사원 A는 매일 커피를 주문하고 커피를 잘 받았다. 누가 만드느냐에 상관없이 원하는 커피를 얻을 수 있었다. 
>
> `커피 주문`이라는 인터페이스를 잘 지키고 있으므로 회사원 A도 커피를 받을 수 있고, 점원 B,C라는 객체도 격일로 변경할 수 있는 것이다!

<br><br>

### 인터페이스 정의

물리적인 형태는 클래스와 동일하다. `.java`형태로 작성되어 컴파일하면 `.class`로 바이트코드가 나타난다.

<br>

`class`키워드 대신에 `interface`를 사용한다.

```java
interface 인터페이스명{}
```

인터페이스는 **상수와 메소드만을 멤버로 가진다.** 

인터페이스는 객체를 생성할 수 없기에 생성자도 가질 수 없다.

Java8부터 메소드도 다양해졌다.

- 추상메소드
- default 메소드 (Java8~)
- static 메소드 (Java8~)

```java
interface 인터페이스명{
    public static final 타입 상수명=값;
    
    // 추상 메소드
    타입 메소드명(매개변수);
    
    // default 메소드
    default 타입 메소드명(매개변수){
        // 메소드 실행 부분
    }
    
    // static 메소드
    static 타입 메소드명(매개변수){}
}
```

<br>

#### 상수 필드

```java
public static final type VARIABLE = value;
```

인터페이스는 데이터를 저장할 수 없다. 상수 필드만 선언할 수 있다.

<br>

#### 추상 메소드

추상 메소드는 이전에 추상클래스에서 설명했다. 실행 부분은 구현 객체에서 정의한다

<br>

#### 디폴트 메소드

Java8부터 들어온 기능으로, 추상 메소드와 다르게 실행 부분을 가지고 있다. default 메소드의 default는 접근제어자의 개념이 아니다. 디폴트 메소드의 접근제어자는 기본적으로 `public`으로 이를 생략할 수 있다.

<br>

#### 정적 메소드

객체가 없어도 인터페이스만으로 호출이 가능하다. 

<br><br>

### 인터페이스 구현

인터페이스는 실제 객체가 없다. 따라서 인터페이스를 사용하기 위해서는 이를 구현해야 한다. 

구현 객체는 `implements` 키워드를 이용해 구현하며, 인터페이스에서 정의된 추상 메소드를 재정의해야 한다.

```java
public class ClassName implements InterfaceName{
    
}
```

예를 통해 이해하자. 

`RemoteControl`이라는 인터페이스가 있다. 

```java
public interface RemoteControl {
    public static final int MAX_VOLUME = 10;
    public static final int MIN_VOLUME = 0;

    static void changeBattery() {
        System.out.println("Change Battery");
    }

    void turnOn();

    void turnOff();

    void setVolume(int volume);

    default void setMute(boolean mute) {
        if (mute) {
            System.out.println("Mute ON");
        } else {
            System.out.println("Mute OFF");
        }
    }
}
```

`RemoteControl`를 구현하는 `Television`클래스를 만들어보자.

```java
public class Television implements RemoteControl {

    private int volume;

    @Override
    public void turnOn() {
        System.out.println("Turn on TV");
    }

    @Override
    public void turnOff() {
        System.out.println("Turn off TV");
    }

    @Override
    public void setVolume(int volume) {
        if (volume > RemoteControl.MAX_VOLUME)
            this.volume = RemoteControl.MAX_VOLUME;
        else if (volume < RemoteControl.MIN_VOLUME)
            this.volume = RemoteControl.MIN_VOLUME;
        else
            this.volume = volume;
        System.out.println("Now TV volume: " + volume);
    }
}

```

다음과 같이 추상메서드를 재정의하여 사용한다. 

<br>

상속 파트에서 자식 객체를 부모 객체 타입으로 명시하여 사용한 케이스를 설명했다. 

인터페이스도 똑같이 사용할 수 있다. 

```java
RemoteControl rc = new Television();
```

물론 메서드를 호출하면 `Television` 객체의 메서드를 사용한다.

<br><br>

### 익명 구현 객체

지금까지는 인터페이스를 구현한 클래스를 선언했다. 하지만, 꼭 이 방법만 있는 것은 아니다. 

다음처럼 구현 객체를 익명으로 만들 수 있다. 

```java
RemoteControl rc = new RemoteControl() {
    
    private int volume;
    
    @Override
    public void turnOn() {
        System.out.println("Turn on");
    }
    
    @Override
    public void turnOff() {
        System.out.println("Turn off");
    }
    
    @Override
    public void setVolume(int volume) {
        this.volume = volume;
    }
};
```

왜 익명이냐?

기존에는 특정 클래스를 선언했다. 이는 클래스의 이름이 있다. 따라서 구현 객체가 특정된다. 

하지만 위의 코드에서는 선언과 동시에  `RemoteControl`타입의 `rc`변수에 할당된다. 구현하는 특정 객체가 없기 때문에 익명이다!

<br><br>

### 인터페이스 상속

보통 상속이라 하면 2가지를 의미한다.

- 클래스 상속 (`extends`)
- 인터페이스 상속(구현) (`implements`)

알다시피 java는 다중 상속을 지원하지 않는다. 정확히는 클래스의 다중 상속을 지원하지 않는다. 

**인터페이스 다중 상속은 지원**한다. 

- 인터페이스가 인터페이스를 다중 상속 가능
- 클래스가 인터페이스를 다중 구현 가능

따라서 아래와 같은 코드가 가능하다. 

```java
interface A{
    void a();
}

interface B extends A{
    void b();
}

interface C{
    void c();
}

class Example implements B,C{
    
    @Override
    public void a() {}

    @Override
    public void b() {}

    @Override
    public void c() {}
}
```

인터페이스 B는 A를 상속한다. 

클래스 `Example`은 인터페이스 `B`,`C`를 구현한다. 따라서 이들의 추상메소드를 모두 구현해야 한다. 

하나 특이하고도 당연한 건 인터페이스 `B`는 인터페이스 `A`를 상속받았으므로 `Example`클래스는 `A`의 추상메소드도 구현해야 한다!

<br>

#### 메서드 시그니처뿐만 아니라 리턴타입을 고려해야 한다.

메서드 시그니처는 같지만, 리턴타입은 다른 메서드를 가진 인터페이스를 모두 구현할 수 있을까?

```java
interface B {
    void b();
}
interface D{
    int b();
}

class Example implements B,D{
    @Override
    public void b() {} // Compile Error!		        
}
```

인터페이스 `B` 와 `D`는 `b()`라는 메소드를 가지고 있지만, 이는 리턴타입이 다르다. 

따라서 Override하려고 하니 컴파일 에러가 발생한다. 

> 메서드 시그니처가 같으니 당연히 오버로딩도 불가하다.

<br>

어떻게 하면 컴파일 에러가 발생하지 않을까?

정답은 리턴타입을 동일하게 해주면 된다. 

```java
interface B {
    void b();
}
interface D{
    void b();
}
```

<br><br>

### Default Method

디폴트 메소드는 인터페이스에 선언된 인스턴스 메소드이기 때문에 구현 객체가 있어야 사용할 수 있다. 

<br>

디폴트 메소드는 왜 생겼을까?

A라는 인터페이스가 있었다. 다양한 객체가 이를 구현하고 있었다. 

그런데 A 인터페이스에서 새로운 기능이 추가됐다. 만약 이 기능을 추상 메서드로 정의한다면? 모든 구현 객체에서 재정의해야 한다.

이는 코드의 변경이 엄청나게 발생하는 사건이다. 

하지만 디폴트 메소드를 사용하면 재정의하지 않아도 된다. 

따라서 기존 클래스는 변경없으면서 새로운 클래스는 디폴트 메소드를 재정의하여 사용할 수 있게 된다.

<br>

#### 디폴트 메소드가 있는 인터페이스 상속

부모 인터페이스에 디폴트 메소드가 정의되어 있을 경우, 자식 인터페이스에서 디폴트 메소드를 활용하는 방법은 3가지다.

- 디폴트 메소드 단순히 상속
- 디폴트 메소드 재정의
- 디폴트 메소드를 추상 메소드로 재선언

<br>

#### 디폴트 메소드 단순 상속

```java
interface Parent{        
    void method1();
    default void method2(){
        System.out.println("method2");
    }
}

interface Child1 extends Parent{
    void method3();
}   

Child1 child1 = new Child1() {
    @Override
    public void method3() {
    }
    
    @Override
    public void method1() {        
    }
}

child1.method2();  // Parent의 method2() 호출
```

이 경우 단순 상속하여 default 메소드를 사용한다.

<br>

#### 디폴트 메소드 재정의

```java
interface Child2 extends Parent{
    @Override
    default void method2(){
        System.out.println("child2 method2");
    }
    
    void method3();
}


Child2 child2 = new Child2() {
    @Override
    public void method3() {}

    @Override
    public void method1() {}
}

child2.method2();	// Child2 인터페이스의 method2() 호출
```

자식 인터페이스는 default 메소드를 재정의하여 사용할 수 있다.

<br>

#### 추상 메서드로 선언

```java
interface Child3 extends Parent{
    @Override
    void method2();
    void method3();
}

Child3 child3 = new Child3() {
    @Override
    public void method2() {    
    }
    
    @Override
    public void method3() {
    }

    @Override
    public void method1() {    
    }
}    
```

default 메소드를 추상 메서드로 재선언하게 되면 구현 객체는 추상 메서드를 구현해야 한다.

<br><br>

### Static Method

인터페이스에서도 static method가 가능하다. 

클래스의 static method와 동일하게 객체를 생성하지 않고 사용할 수 있다. 

인터페이스의 default 메소드와 같이 Java8부터 지원하는데, 왜 사용하는 것일까?

일단 인터페이스의 static method의 특징 2가지를 알고 가자.

1. 함수의 실행 부분이 있어야 한다.
2. Override할 수 없다. 

다른 구현 객체가 static 메소드를 재정의할 수 없다는 것은 

static method가 정의된 인터페이스의 의도대로'만' 메소드를 사용해야 한다는 뜻이다. 

<br>

즉, **다른 구현 객체가 잘못 구현하는 경우를 금지**한다.

그래서 `Collections` 인터페이스의 `sort`같은 메소드를 도구처럼 편하게 사용할 수 있다.

<br><br>

### Private Method

private method가 특별한 건 아니지만, default method가 지원되면서 필요하게 되었다. 

왜냐하면 인터페이스에서 여러 개의 default method를 선언하다보면 중복 코드가 발생할 수 있기 때문이다. 

따라서 private method를 통해 코드의 중복을 해결할 수 있다.

<br><br>

### Functional Interface

직역하면 함수형 인터페이스이다. Functional Interface는 **단 하나의 추상 메서드를 가진 인터페이스**를 Annotation을 통해 명시할 수 있다.

```java
@FunctionalInterface
interface A{
    void print(int val);
}
```

interface A는 `print`라는 함수를 가진다. 

구현은 다른 인터페이스와 동일하다. 

다만 하나의 함수만 구현하면 되므로 람다식을 이용할 수 있다.

1. 기존처럼 구현 클래스/ 익명 클래스를 통해 구현한다.

   ```java
   A a1 = new A() {
       @Override
       public void print(int val) {
           
       }
   };
   ```

2. 람다 이용

   ```java
   A a2 = val->{};
   ```

<br>

<br>

### 추상클래스와 인터페이스

추상 클래스와 인터페이스의 차이점은 무엇일까요? 라는 질문은 단골 질문이다. 

Java 8이전에는 추상 클래스와 인터페이스의 차이점은 분명 있었다. 

그런데 인터페이스가 default, static 메소드를 지원하면서부터 차이점이 줄어들게 되었다. 

하지만 분명 차이점은 있다.

<br>

추상 클래스는 `is-a`, 인터페이스는 `has-a`이다. 

| 추상 클래스             | 인터페이스         |
| ----------------------- | ------------------ |
| `is-a`                  | `has-a`            |
| 객체의 특성을 정의      | 객체의 행위를 정의 |
| 상수, 변수 필드, 메소드 | 상수 필드만 가능   |

- 추상 클래스는 **객체의 특성을 정의**한다. 클래스의 특성을 추상화시켜 이를 상속을 통해 객체를 생성한다. 즉, 생성된 객체는 추상 클래스의 특성을 갖게 된다. 그래서 변수 필드를 가질 수 있다. 특성을 갖고 있기 때문에 `is-a`이다. ex. 학생이라는 클래스는 사람 추상 클래스를 상속한다. 

- 인터페이스는 **객체의 행위를 정의**한다. 객체가 가지고 있는 행위는 다양할 수 있다. 이를 인터페이스로 정의하여 객체간 통신(?)하는 것이다.

  그래서 변수 필드는 존재하지 않으며, 메서드와 상수 필드만 가지고 있다. 또한, 다양한 행위를 정의하기 위해 다중 상속이 가능하다.

  ex. 학생이라는 클래스는 공부라는 인터페이스를 가진다. 



<br><br>

### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어

[JournalDev](https://www.journaldev.com/2752/java-8-interface-changes-static-method-default-method#java-interface-static-method)



