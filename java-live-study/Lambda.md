# Lambda

<br>

### Lambda?

Java는 함수형 프로그래밍을 위해 Java 8부터 람다식(Lambda Expressions)을 지원한다. 

람다식은 익명 함수(anonymous function)를 생성하기 위한 식으로 객체 지향보다는 함수 지향 언어에 가깝다. 

람다식의 형태는 매개 변수를 가진 코드 블록이지만, 런타임 시에는 익명 구현 객체를 생성한다.

<br>

기존의 인터페이스의 익명 구현 객체를 생성할 때 다음과 같이 작성하였다.

```java
Runnable runnable = new Runnable(){
    @Override
    public void run(){
        //...
    }
}
```

람다식으로 표현하면 다음과 같이 줄어든다.

```java
Runnable runnable = () -> { /*...*/ };
```

<br>

람다식의 문법은 다음과 같다.

```java
(타입 매개변수,...) -> { 실행문; }
```

```java
(int a) -> { System.out.println(a);}

// 매개변수 타입은 런타임 시에 타입 추론 => 생략 가능
(a, b) -> { System.out.println(a + b);}

// 매개변수가 하나라면 괄호 생략 가능
a -> System.out.println(a);

// 매개변수가 없다면 빈 괄호 반드시 사용한다.
() -> { System.out.println();}

// 결과 반환해야 한다면 return문 사용
(x, y) -> { return x+y; };

// 중괄호 return문만 있을 경우, return문을 사용하지 않는다.
(x, y) -> x+y
```

<br><br>

### Target Type

람다식은 익명 구현 객체를 생성한다. 그렇다면 어떤 타입의 객체를 생성할까?

```java
인터페이스 변수 = 람다식;
```

이처럼 람다식은 인터페이스 변수에 대입된다. 

람다식은 대입될 인터페이스의 종류에 따라 작성 방법이 달라지기 때문에 람다식이 대입될 인터페이스를 람다식의 타겟 타입(target type)이라고 한다.

<br>

#### @FunctionalInterface

람다식은 인터페이스를 타겟 타입으로 하지만, 모든 인터페이스가 이에 해당하는 것은 아니다. 

람다식은 하나의 메서드를 정의하기 때문에 **오직 하나의 추상 메서드만 선언된 인터페이스만 람다식의 타겟 타입**이 된다. 

인터페이스에서 언급했지만, 이처럼 하나의 추상 메서드만 선언된 인터페이스를 함수적 인터페이스라고 한다.

이는 `@FunctionalInterface` 애노테이션을 통해 컴파일러가 체크해준다. 

<br>

이제 여러 가지 상황을 통해 람다식을 사용해보자.

1. 매개 변수와 리턴 값이 없는 람다식

   ```java
   @FunctionalInterface
   public interface MyFunctionalInterface{
       public void method();
   }
   
   MyFunctionalInterface fi = () ->  {};
   // 람다식을 변수에 저장하는 것만으로 실행이 되지 않는다.
   fi.method();
   ```

2. 매개 변수가 있지만, 리턴값이 없는 람다식

   ```java
   @FunctionalInterface
   public interface MyFunctionalInterface{
       public void method(int x);
   }
   
   MyFunctionalInterface fi = x -> { System.out.println(x) };
   fi.method(5); // 5
   ```

3. 리턴값이 있는 람다식

   ```java
   @FunctionalInterface
   public interface MyFunctionalInterface{
       public void method(int x, int y);
   }
   
   MyFunctionalInterface fi = (x, y) -> x + y;
   fi.method(5, 3); // 8
   
   public int multiply(int a, int b){
       return a * b;
   }
   
   MyFunctionalInterface fi = (x, y) -> multiply(x, y);
   fi.method(5, 3); // 15
   ```

<br><br>

### 익명 구현 객체

위에서 람다는 익명 구현 객체를 생성한다고 했다. 

그런데 익명 구현 객체를 직접 구현하는 것과 람다식으로 표현하는 법의 차이는 무엇일까?

당연히 컴파일러가 람다식을 확인해 이를 익명 구현 객체과 동일하게 만들어준다고 생각했다. 

그래서 다음과 같은 클래스를 실행해봤다.

```java
@FunctionalInterface
public interface MyFunctionalInterface {
    void method();
}

public class LambdaTest {

    public static void main(String[] args) {
        LambdaTest lambdaTest = new LambdaTest();

        lambdaTest.method1();
        lambdaTest.method2();

    }
    // 람다식
    public void method1() {
        MyFunctionalInterface fi1 = ()->{
            System.out.println("it's fi1");
        };
        fi1.method();
    }
    // 익명 구현 객체
    public void method2() {
        MyFunctionalInterface fi2 = new MyFunctionalInterface() {
            @Override
            public void method() {
                System.out.println("it's fi2");
            }
        };
        fi2.method();
    }
}
```

그래서 decompiled된 class를 봤다. 

```java
public class LambdaTest {
    public LambdaTest() {
    }

    public static void main(String[] args) {
        LambdaTest lambdaTest = new LambdaTest();
        lambdaTest.method1();
        lambdaTest.method2();
    }

    public void method1() {
        MyFunctionalInterface fi1 = () -> {
            System.out.println("it's fi1");
        };
        fi1.method();
    }

    public void method2() {
        MyFunctionalInterface fi2 = new MyFunctionalInterface() {
            public void method() {
                System.out.println("it's fi2");
            }
        };
        fi2.method();
    }
}
```

<br>

이전과 뭐가 다른지 잘 모르겠어서 이젠 바이트 코드를 봤다.

<img src="https://github.com/gashe-soo/java-study/blob/master/img/lj_lambda_byte1.JPG?raw=true" alt="lj_lambda_byte1.jpg"  />

<img src="https://github.com/gashe-soo/java-study/blob/master/img/lj_lambda_byte2.JPG?raw=true" alt="lj_lambda_byte2.jpg"  />

보면 `method1()`과 `method2()`의 빨간 부분이 분명히 다르다. 솔직히 말하면 내부 구현이 어떻게까지 되는지는 모르겠다. 

확실한 것은 람다식은 익명 구현 객체와 100퍼센트 동일하게 만들어지진 않는다는 것이다.

<br>

> 참고. 람다식의 내부 동작을 알아보자.
>
> [링크1](https://tourspace.tistory.com/11)
>
> [링크2](https://tourspace.tistory.com/12?category=788398)



그럼 어떻게 다른지도 조금 더 살펴보자.

<br><br>

### 클래스 멤버와 로컬 변수

람다식의 실행 블록에는 클래스의 멤버 및 로컬 변수를 사용할 수 있다. 

<br>

#### 클래스의 멤버

클래스의 필드와 메서드를 제약 사항 없이 사용할 수 있다. 하지만 `this`키워드를 사용할 때에는 주의가 필요하다. 

```java
@FunctionalInterface
public interface MyFunctionalInterface {
    void method();
}

public class UsingThis {
    public int outterField = 10;

    class Inner{
        int innerField = 20;

        void method(){
            MyFunctionalInterface fi = ()->{
                System.out.println("outterField = " + outterField);
                System.out.println("UsingThis.this.outterField = " + UsingThis.this.outterField);

                System.out.println("innerField = " + innerField);
                System.out.println("this.innerField = " + this.innerField);	// this는 Inner 객체

            };
            fi.method();
        }
    }

    public static void main(String[] args) {
        UsingThis usingThis = new UsingThis();
        UsingThis.Inner inner = new UsingThis().new Inner();
        inner.method();
    }
}

// 결과 
outterField = 10
UsingThis.this.outterField = 10
innerField = 20
this.innerField = 20
```

그렇다면 필드명이 똑같다면?

```java
public class UsingThis {
    public int field = 10;

    class Inner{
        int field = 20;

        void method(){
            MyFunctionalInterface fi = ()->{
                System.out.println("field = " + field);
                System.out.println("UsingThis.this.field = " + UsingThis.this.field);
                System.out.println("this.field = " + this.field);

            };
            fi.method();
        }
    }
}

// 결과
field = 20
UsingThis.this.field = 10
this.field = 20
```

람다식이 선언된 객체의 필드가 우선이다. 

<br>

#### 로컬 변수

문제는 로컬 변수다. 

자바에서는 [Shadowing](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html#shadowing)을 사용한다. 

> Shadowing
>
> 같은 이름을 가진 변수가 범위마다 있다면 제일 좁은 범위의 변수가 다른 범위의 변수를 가린다. 
>
> 예를 들어 메서드의 변수는 같은 이름의 클래스 변수를 가린다. 예시는 [shadowing](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html#shadowing)참고

그렇다면 람다식에서는 어디까지 사용할 수 있을까?

<br>

먼저, 익명 구현 객체를 테스트해보자. 아래와 같이 선언했다. 

```java
public void method2() {
    int field = 30;
    MyFunctionalInterface fi2 = new MyFunctionalInterface() {
        int field = 40;
        @Override
        public void method() {
            System.out.println(field);
        }
    };
    fi2.method();
}
```

`method2()`를 실행하면 결과는 40이 출력된다. 익명 구현 객체에 지역 변수를 선언하고 이를 사용할 수 있다. 즉, 쉐도잉이 잘 동작한다.

그렇다면 익명 구현 객체의 `method()` 내부엔 가능할까?

결론은 가능하다. 

<br>

반면에, 람다식은 어떻게 동작할까?

람다식 표현 내부에 지역 변수를 선언해봤다. 그랬더니 컴파일 에러가 발생했다.

```java
public void method1() {
    int field = 20;
    MyFunctionalInterface fi1 = ()->{
        int field = 30;	// compile error!
        System.out.println(field);
    };
    fi1.method();
}
```

경고문을 봤더니 이미, `field`가 정의되었다고 한다. 지역 변수가 아닌 매개 변수도 동일하다.

따라서 람다식은 선언된 메서드의 변수를 쉐도잉하지 못하고 있다.

<br>

즉, 람다식의 변수는 람다식이 선언된 메서드의 범위를 가지고 있다.

<br>

#### Variable Capture

지역 변수의 얘기를 좀 더 해보자. 

기본적으로 지역 변수는 메소드 실행이 끝나면 스택 메모리에서 사라진다. 그런데 익명 구현 객체는 생성되면 힙 메모리에 존재해서 계속 사용할 수 있다. 

그러면 익명 구현 객체에서 메소드의 지역 변수를 사용할 수 있을까? 

사용할 수 없다.

그래서 자바는 이 문제를 해결하기 위해 컴파일 시 사용하는 매개변수나 지역 변수의 값을 익명 구현 객체 내부에 복사해두고 사용한다. 

그리고 지역 변수가 수정되어 값이 변경되면 클래스 내부 복사값과 달라지게 된다. 그래서 이를 해결하기 위해 지역 변수를 `final`로 선언해서 수정을 막는다. 

<br>

자바 7이전까지는 반드시 `final` 키워드로 변수들을 선언해야 했지만, 자바 8부터는 `final` 키워드 없이도 `final` 특성을 갖도록 했다.

```java
public void method1() {
    int field = 20;
    field = 30;
    MyFunctionalInterface fi1 = ()->{
        System.out.println(field);	// compile error! (Variable used in lambda expression should be final or effectively final)
    };
    fi1.method();
}
```

<br><br>

### 표준 API 함수형 인터페이스

자바 8부터는 빈번하게 사용되는 함수형 인터페이스는 `java.util.function` 패키지로 제공한다. 

| 종류      | 추상 메서드 특징                                             |
| --------- | ------------------------------------------------------------ |
| Consumer  | 매개값은 있고, 리턴 값은 없음                                |
| Supplier  | 매개값은 없고, 리턴값은 있음                                 |
| Function  | 매개값, 리턴값 있음<br />주로 매개값을 리턴값으로 매핑       |
| Operator  | 매개값, 리턴값 있음<br />주로 매개값을 연산하고 결과를 리턴  |
| Predicate | 매개값은 있고, 리턴 타입은 `boolean`<br />매개값을 조사해서 true/false를 리턴 |

<br>

#### Consumer

```java
@FunctionalInterface
public interface Consumer<T> {

    /**
     * Performs this operation on the given argument.
     *
     * @param t the input argument
     */
    void accept(T t);

    /**
     * Returns a composed {@code Consumer} that performs, in sequence, this
     * operation followed by the {@code after} operation. If performing either
     * operation throws an exception, it is relayed to the caller of the
     * composed operation.  If performing this operation throws an exception,
     * the {@code after} operation will not be performed.
     *
     * @param after the operation to perform after this operation
     * @return a composed {@code Consumer} that performs in sequence this
     * operation followed by the {@code after} operation
     * @throws NullPointerException if {@code after} is null
     */
    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}
```

Consumer는 매개값을 소비하고 리턴 값은 없는 `accept()`메서드를 가지고 있다. 

| 인터페이스명     | 추상 메서드            | 설명                   |
| ---------------- | ---------------------- | ---------------------- |
| Consumer<T>      | void accept(T t)       | 객체 T를 받아 소비     |
| BiConsumer<T, U> | void accept(T t, U u)  | 객체 T와 U를 받아 소비 |
| IntConsumer      | void accept(int value) | int 값을 받아 소비     |

이외에도 다양한 Consumer가 존재한다. 형태는 `XXXConsumer`다.

<br>

#### Supplier

Supplier는 매개 변수가 없고 리턴값이 있는 `getXXX()`메서드를 가진다.

```java
@FunctionalInterface
public interface Supplier<T> {

    /**
     * Gets a result.
     *
     * @return a result
     */
    T get();
}
```

```java
Supplier<String> supplier = () -> "문자열";
```

<br>

#### Function

Function 함수적 인터페이스는 매개값과 리턴값이 있는 `applyXXX()`메서드를 가진다. 

```java
@FunctionalInterface
public interface Function<T, R> {

    /**
     * Applies this function to the given argument.
     *
     * @param t the function argument
     * @return the function result
     */
    R apply(T t);

    /**
     * Returns a composed function that first applies the {@code before}
     * function to its input, and then applies this function to the result.
     * If evaluation of either function throws an exception, it is relayed to
     * the caller of the composed function.
     *
     * @param <V> the type of input to the {@code before} function, and to the
     *           composed function
     * @param before the function to apply before this function is applied
     * @return a composed function that first applies the {@code before}
     * function and then applies this function
     * @throws NullPointerException if before is null
     *
     * @see #andThen(Function)
     */
    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }

    /**
     * Returns a composed function that first applies this function to
     * its input, and then applies the {@code after} function to the result.
     * If evaluation of either function throws an exception, it is relayed to
     * the caller of the composed function.
     *
     * @param <V> the type of output of the {@code after} function, and of the
     *           composed function
     * @param after the function to apply after this function is applied
     * @return a composed function that first applies this function and then
     * applies the {@code after} function
     * @throws NullPointerException if after is null
     *
     * @see #compose(Function)
     */
    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

    /**
     * Returns a function that always returns its input argument.
     *
     * @param <T> the type of the input and output objects to the function
     * @return a function that always returns its input argument
     */
    static <T> Function<T, T> identity() {
        return t -> t;
    }
}
```

```java
Fuction<String,String> func = t -> t.toUpperCase();
```

매개값을 2개 받는 `BiFunction<T,U,R>`도 있다. 

<br>

#### Operator

Operator 함수적 인터페이스는 `Function`과 동일하게 매개 변수와 리턴값이 있는 `applyXXX()`메서드를 가진다. 다만 `Function`과 다른 점은 매개값과 리턴값의 타입이 같다. 

`Operator`는 매개값의 개수에 따라 `BinaryOperator<T>`, `UnaryOperator<T>` 등이 있다. 그리고 매개값의 타입이 앞에 붙기도 한다. ex. `IntBinaryOperator`

아래는 `BinaryOperator<T>`의 코드다.

```java
@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T,T,T> {
    /**
     * Returns a {@link BinaryOperator} which returns the lesser of two elements
     * according to the specified {@code Comparator}.
     *
     * @param <T> the type of the input arguments of the comparator
     * @param comparator a {@code Comparator} for comparing the two values
     * @return a {@code BinaryOperator} which returns the lesser of its operands,
     *         according to the supplied {@code Comparator}
     * @throws NullPointerException if the argument is null
     */
    public static <T> BinaryOperator<T> minBy(Comparator<? super T> comparator) {
        Objects.requireNonNull(comparator);
        return (a, b) -> comparator.compare(a, b) <= 0 ? a : b;
    }

    /**
     * Returns a {@link BinaryOperator} which returns the greater of two elements
     * according to the specified {@code Comparator}.
     *
     * @param <T> the type of the input arguments of the comparator
     * @param comparator a {@code Comparator} for comparing the two values
     * @return a {@code BinaryOperator} which returns the greater of its operands,
     *         according to the supplied {@code Comparator}
     * @throws NullPointerException if the argument is null
     */
    public static <T> BinaryOperator<T> maxBy(Comparator<? super T> comparator) {
        Objects.requireNonNull(comparator);
        return (a, b) -> comparator.compare(a, b) >= 0 ? a : b;
    }
}
```

<br>

#### Predicate

`Predicate` 함수적 인터페이스 매개변수와 boolean 리턴값이 있는 `testXXX()` 메서드를 가진다. 매개값을 조사해서 boolean값을 리턴한다.

```java
@FunctionalInterface
public interface Predicate<T> {

    /**
     * Evaluates this predicate on the given argument.
     *
     * @param t the input argument
     * @return {@code true} if the input argument matches the predicate,
     * otherwise {@code false}
     */
    boolean test(T t);

    /**
     * Returns a composed predicate that represents a short-circuiting logical
     * AND of this predicate and another.  When evaluating the composed
     * predicate, if this predicate is {@code false}, then the {@code other}
     * predicate is not evaluated.
     *
     * <p>Any exceptions thrown during evaluation of either predicate are relayed
     * to the caller; if evaluation of this predicate throws an exception, the
     * {@code other} predicate will not be evaluated.
     *
     * @param other a predicate that will be logically-ANDed with this
     *              predicate
     * @return a composed predicate that represents the short-circuiting logical
     * AND of this predicate and the {@code other} predicate
     * @throws NullPointerException if other is null
     */
    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }

    /**
     * Returns a predicate that represents the logical negation of this
     * predicate.
     *
     * @return a predicate that represents the logical negation of this
     * predicate
     */
    default Predicate<T> negate() {
        return (t) -> !test(t);
    }

    /**
     * Returns a composed predicate that represents a short-circuiting logical
     * OR of this predicate and another.  When evaluating the composed
     * predicate, if this predicate is {@code true}, then the {@code other}
     * predicate is not evaluated.
     *
     * <p>Any exceptions thrown during evaluation of either predicate are relayed
     * to the caller; if evaluation of this predicate throws an exception, the
     * {@code other} predicate will not be evaluated.
     *
     * @param other a predicate that will be logically-ORed with this
     *              predicate
     * @return a composed predicate that represents the short-circuiting logical
     * OR of this predicate and the {@code other} predicate
     * @throws NullPointerException if other is null
     */
    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }

    /**
     * Returns a predicate that tests if two arguments are equal according
     * to {@link Objects#equals(Object, Object)}.
     *
     * @param <T> the type of arguments to the predicate
     * @param targetRef the object reference with which to compare for equality,
     *               which may be {@code null}
     * @return a predicate that tests if two arguments are equal according
     * to {@link Objects#equals(Object, Object)}
     */
    static <T> Predicate<T> isEqual(Object targetRef) {
        return (null == targetRef)
                ? Objects::isNull
                : object -> targetRef.equals(object);
    }

    /**
     * Returns a predicate that is the negation of the supplied predicate.
     * This is accomplished by returning result of the calling
     * {@code target.negate()}.
     *
     * @param <T>     the type of arguments to the specified predicate
     * @param target  predicate to negate
     *
     * @return a predicate that negates the results of the supplied
     *         predicate
     *
     * @throws NullPointerException if target is null
     *
     * @since 11
     */
    @SuppressWarnings("unchecked")
    static <T> Predicate<T> not(Predicate<? super T> target) {
        Objects.requireNonNull(target);
        return (Predicate<T>)target.negate();
    }
}

```

```java
Predicate<String> predicate = s -> s.equals("HelloWorld");
```

<br>

#### andThen(), compose()

`Consumer`, `Function`, `Operator` 종류의 함수형 인터페이스는 `andThen()`, `compose()` 라는 default 메서드를 가진다. 

동일한 함수형 인터페이스만 `andThen()`, `compose()` 의 매개 값으로 올 수 있다. 

- `andThen()`

  - ```java
    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }
    ```

    (`Function`은 예시다.)

    `andThen`메서드는 호출한 람다식을 먼저 실행한다. 람다식의 결과값을 매개값으로 받은 람다식의 매개값으로 제공한다.

    매개 값의 람다식 결과가 최종 결과다.

- `compose()` 

  - `compose()`메서드는 `andThen()`과 반대로 동작한다. 매개값의 람다식을 먼저 실행하고 결과값을 메서드 호출한 람다식의 매개값으로 제공한다.

  - ```java
    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }
    ```

<br>

<br>

### Method Reference

메서드 참조는 메소드를 참조해서 매개 변수의 정보 및 리턴 타입을 알아내어, 람다식에서 불필요한 매개 변수를 제거하는 것이 목적이다. 

예시를 보자.

```java
// 두 개 중 큰 값을 반환하는 람다식
(left, right) -> Math.max(left, right);
```

람다식은 단순히 메서드의 매개값을 전달하는 역할을 한다. 이런 경우 메서드 참조를 사용한다.

```java
IntBinaryOperator operator = Math::max;
```

<br>

메서드 참조에는 3가지 방식이 있다.

<br>

#### 정적 메서드와 인스턴스 메서드 참조

정적 메서드를 참조할 경우, 다음과 같이 사용한다.

```java
클래스::메서드
    
    
public class Calculator{
    public static int staticMethod(int x, int y){
        return x + y;
    }
    public int instanceMethod(int x, int y){
        return x + y;
    }
}

// 기존
IntBinaryOperator operator = (x, y) -> Calculator.staticMethod(x, y);
// 메서드 참조
operator = Calculator::staticMethod;
```

인스턴스 메서드일 경우, 객체를 생성한다음, 참조 변수 뒤에 :: 기호를 붙이고 인스턴스 메서드 이름을 기술한다.

```java
참조변수::메서드
    
Calculator calculator = new Calculator();
// 기존
IntBinaryOperator operator = (x, y) -> calculator.instanceMethod(x,y);
// 메서드 참조
operator = calculator::instanceMethod;
```

<br>

#### 매개변수의 메서드 참조

메서드는 람다식에서 제공되는 매개 변수의 멤버일수도 있다. 그래서 이를 사용하려면 다음과 같이 작성했다.

```java
(a, b) -> { a.instanceMethod(b); }
```

이를 메서드 참조로 표현하면 다음과 같다. a 클래스 이름 뒤에 메서드 이름을 기술한다. 

방법은 정적 메서드와 같지만, a의 인스턴스 메서드가 참조된다.

```java
클래스::instanceMethod
```

```java
// 기존
ToIntBinaryFunction<String,String> function = (a, b) -> a.compareToIgnoreCase(b);
// 메서드 참조
function = String::compareToIgnoreCase;
```

<br>

#### 생성자 참조

메서드 참조는 생성자 참조도 포함된다. 

```java
(a, b) -> new Class(a,b);
```

이를 생성자 참조로 표현할 수 있다. 

```java
클래스::new
```

만약 생성자가 오버로딩되어 있을 경우, 컴파일러는 동일한 매개변수 타입과 개수를 가지고 있는 생성자를 찾아 실행한다. 

만약 해당 생성자가 존재하지 않으면 컴파일 오류가 발생한다.

<br>

<br>

### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어

