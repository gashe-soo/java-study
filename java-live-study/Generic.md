# Generic

<br>

### 제네릭을 쓰는 이유

제네릭은 클래스, 인터페이스, 메소드를 정의할 때 type이 parameter가 될 수 있도록 한다. 

메서드 선언에 사용되는 기존의 파라미터와 유사하게, 타입 파라미터는 다른 입력에도 같은 코드를 재사용할 수 있도록 한다.

기존에는 값이었지만, 제네릭은 타입 파라미터를 사용한다는게 차이점일뿐이다.

제네릭을 쓰면 좋은 점은

- 컴파일 타임에 강한 타입 체크
  - Java 컴파일러가 강한 타입 체크를 할 수 있으며, 코드가 type safety하지 않을 경우 에러를 발생시킨다. 
  - 컴파일 타임 에러를 고치는 것이 찾기 힘든 런타임 에러를 고치는 것보다 쉽다.

- Cast 제거

  - 제네릭을 사용하지 않는 코드는 casting이 필요하다.

    - ```java
      List list = new ArrayList();
      list.add("hello");
      String s = (String) list.get(0);
      ```

  - 제네릭은 casting이 필요없다.

    - ```java
      List<String> list = new ArrayList<String>();
      list.add("hello");
      String s = list.get(0);   // no cast
      ```

- 프로그래머가 제네릭 알고리즘 구현
  - 제네릭을 사용하기에, 프로그래머는 다른 타입들에게 적용되는 알고리즘을 구현할 수 있다. 
  - 이는 type safe 하고 가독성이 좋다.

<br><br>

### Generic Type

*generic type*은 type을 parameter화한 클래스나 인터페이스를 뜻한다. 

예시를 보면서 이해하자.

다음과 같이 제네릭을 사용하지 않은 `Box`클래스가 있다고 하자. 이는 `set`,`get`메서드만 제공한다.

```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

메서드가 `Object` 클래스를 받고 리턴하므로, 원시타입만 아니면 무엇을 pass하든 상관없다. 

그러나 컴파일 타임엔 어떤 클래스가 쓰이는 지를 검증할 수 없다. 

```java
Box box = new Box();
box.set("123");
Integer val = (Integer)box.get();
```

원래는 `Integer`타입을 사용하려고 했는데 실수로 `String`타입을 넣어도 아무런 오류가 발생하지 않는다. 따라서 런타임에나 오류를 확인하게 된다.

그러면 제네릭을 사용해보자.

<br>

#### A Generic Version of the Box Class

제네릭은 다음과 같이 정의한다. 

```java
class name<T1, T2, ..., Tn> { /* ... */ }
```

type parameter에서는 `<>` 표시를 통해 사용할 클래스 이름을 담는다. 

제네릭을 사용하면 `Box` class는 다음과 같이 변한다.

```java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

이전의 `Object` 부분은 모두 `T`로 대체되었다. 타입 변수는 원시타입이 아니어야 한다. (클래스, 인터페이스, 배열 등)

제네릭 인터페이스도 동일하게 적용된다.

<br>

#### Type Parameter Naming Conventions

컨벤션에 따르면 type parameter는 하나의 대문자이다. 

- E - Element (used extensively by the Java Collections Framework)
- K - Key
- N - Number
- T - Type
- V - Value
- S,U,V etc. - 2nd, 3rd, 4th types

<br>

#### 제네릭 타입 객체 생성 및 호출

generic type를 호출할 때는 `T`대신 특정한 타입을 명시한다.

```java
Box<Integer> integerBox;
```

일반적인 메서드 호출과 비슷하다고 보이지만, 메서드에 인자를 전달하는 대신 제네릭에선 *type argument*를 전달한다. 

> Type parameter vs Type argument
>
> Type parameter과 Type argument는 다르다. 
>
> 예를 들어, Box<T>에서 T는 type parameter고 실제로 호출하는 과정 속에서 Box<Integer>에서 Integer는 type argument다.

<br>

제네릭 타입의 객체를 생성하기 위해서는 다음과 같이 선언한다.

```java
Box<Integer> integerBox = new Box<Integer>();
```

자바 7이후 부터는 *다이아몬드*라고 불리는 꺾쇠 괄호(<>)안에 타입 인자를 명시하지 않아도 컴파일러가 인식해서 컴파일한다.

```java
Box<Integer> integerBox = new Box<>();
```

<br>

#### Multiple Type Parameters

제네릭 클래스는 여러 type parameter를 가질 수 있다. 인터페이스도 동일하다.

```java
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}
```

두 개의 OrderedPair 구현체를 생성할 수 있다.

```java
Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");
```

위처럼 구현에 따라 K, V를 설정할 수 있다. `String`,`Integer`를 사용해도 autoboxing이 일어나기 때문에 `String`, `int`를 생성자에서 사용할 수 있다.

위에서 언급했듯이 객체 생성시 다이아몬드 안에 타입을 생략해도 된다.

```java
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");
```

또한, type parameter에 parameterized type를 사용할 수 있다.

```java
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...));
```

<br><br>

### Raw Types

*raw type*이란 제네릭 클래스에서 type argument가 없는 경우다. 

```java
public class Box<T> {
    public void set(T t) { /* ... */ }
    // ...
}
```

기존에는 type argument를 명시했다.

```java
Box<Integer> intBox = new Box<>();
```

type argument이 생략되면 이는 `Box<T>`의 raw type이다.

```java
Box rawBox = new Box();
```

raw type은 제네릭 클래스이 타입 인자를 생략할 경우다. 따라서 non-generic class가 raw type은 아니다.

이는 제네릭이 나타나기 전에 많이 쓰였다. 

raw type은 모든 타입을 `Object`로 판단한다.

다음과 같이 raw type에 기존 generic type를 할당할 수 있다.

```java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;               // OK
```

반면 그 반대는 불가능하다. type이 다를 수 있기 때문이다.

```java
Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion
```

```java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)
```

raw type이 generic type check를 우회하기 때문에, 런타임에 안전하지 않은 코드가 실행될 수 있다는 것을 경고한다. 

따라서 raw type를 쓰지 않아야 한다.

<br><br>

### Unchecked Error Messages

legacy code와 제네릭을 혼용하는 것은 다음과 같은 경고를 발생시킨다. 

```java
Note: Example.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
```

이는 raw type에서 실행되는 오래된 API를 사용할 때 발생한다. 

```java
public class WarningDemo {
    public static void main(String[] args){
        Box<Integer> bi;
        bi = createBox();
    }

    static Box createBox(){
        return new Box();
    }
}
```

`unchecked`는 컴파일러가 type safety를 보장하기 위해 필요한 타입 체크를 실행하는데, 이를 실행할 수 있는 충분한 타입 정보가 없다는 것을 뜻한다. 

기본적으로 `unchecked` 경고는 컴파일러가 힌트를 주지만, 비활성화되어 있다. 

이를 확인하기 위해서는  `-Xlint:unchecked` 옵션을 사용해 재컴파일하자.

그러면 다음과 같은 경고가 나타난다.

```java
WarningDemo.java:4: warning: [unchecked] unchecked conversion
found   : Box
required: Box<java.lang.Integer>
        bi = createBox();
                      ^
1 warning
```

<br><br>

### Generic Methods

*Generic methods*은 자신의 type parameter를 가진 메서드이다. 제네릭 타입 선언과 비슷하지만, type parameter의 범위가 메서드에 한정된다. 

정적, 비정적 메소드가 있다.

정적 메서드의 경우 리턴타입 앞에 타입 매개변수가 있어야 한다.

`Util`이란 클래스는 정적 제네릭 메서드를 가진다.

```java
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}

public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}
```

다음과 같이 사용한다.

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);
```

호출할 때는 생략해도 컴파일러가 필요할 경우 추론한다.

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.compare(p1, p2);
```

<br><br>

### Bounded Type Parameters

타입 인자가 제한된 타입을 사용하기를 원할 수 있다. 예를 들어 특정 메서드는 특정 클래스 및 하위 클래스만 타입 인자로 받는다. 이럴때 bounded type parameter를 사용한다. 

type parameter를 선언할 때 `extends` 키워드를 붙여 사용한다. 이는 `extends` 뒤에 오는 클래스 및 하위 클래스만 인자로 전달받는다는 것이다. 

여기서 `extends`는 상속/ 구현 모두를 포함한다.

```java
public class Box<T> {

    private T t;          

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }

    public <U extends Number> void inspect(U u){
        System.out.println("T: " + t.getClass().getName());
        System.out.println("U: " + u.getClass().getName());
    }

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<Integer>();
        integerBox.set(new Integer(10));
        integerBox.inspect("some text"); // error: this is still String!
    }
}
```

String을 인자로 넘겼기에 실패한다.

```java
Box.java:21: <U>inspect(U) in Box<java.lang.Integer> cannot
  be applied to (java.lang.String)
                        integerBox.inspect("10");
                                  ^
1 error
```

제네릭 타입에도 bounded type parameter를 사용할 수 있는데, 메서드에도 똑같은 타입이 적용된다.

```java
public class NaturalNumber<T extends Integer> {

    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return n.intValue() % 2 == 0;
    }

    // ...
}
```

<br>

#### Multiple Bounds

하나의 클래스 뿐만 아니라 여러 클래스로 bound할 수 있다.

```java
<T extends B1 & B2 & B3>
```

<br>

만약 multiple bound 중 하나만 클래스라면 이를 가장 처음에 설정해줘야 한다. 그렇지 않으면 컴파일 에러가 발생한다.

```java
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }
class D <T extends B & A & C> { /* ... */ }  // compile-time error
```

<br><br>

### Generic Methods and Bounded Type Parameters

Bounded type parameters는 제네릭 알고리즘 구현에 중요하다.  

`array T[]`에서 `elem`보다 큰 원소를 세는 메서드를 생각해보자.

```java
public static <T> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e > elem)  // compiler error
            ++count;
    return count;
}
```

연산자 `>`는 원시타입에만 적용되므로 컴파일 에러가 발생한다. 

객체를 비교하기 위해서는 `Comparable<T>` 인터페이스의 `compareTo`메서드를 사용해야 한다. 

따라서 타입 매개변수를 `Comparable<T>` 구현한 경우로 생각해보자.

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

코드는 다음과 같이 변경된다.

```java
public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e.compareTo(elem) > 0)
            ++count;
    return count;
}
```

<br>

<br>

### Generics, Inheritance, and Subtypes

알다시피, 아래 예시처럼 상위 클래스에 하위 클래스를 할당할 수 있다. 

```java
Object someObject = new Object();
Integer someInteger = new Integer(10);
someObject = someInteger;   // OK
```

다음과 같이 메서드에서도 사용할 수 있다.

```java
public void someMethod(Number n) { /* ... */ }

someMethod(new Integer(10));   // OK
someMethod(new Double(10.1));   // OK
```

이는 제네릭에도 적용된다. 

타입 인자가 `Number`이지만, 여기에 `Integer`를 add할 수 있다.

```java
Box<Number> box = new Box<Number>();
box.add(new Integer(10));   // OK
box.add(new Double(10.1));  // OK
```

다음 메서드를 보면 `Box<Number>`를 타입 매개변수로 전달받는다.

```java
public void boxTest(Box<Number> n) { /* ... */ }
```

이전처럼 생각해보면 `Box<Number>`가 매개변수이므로 `Box<Integer>`도 가능하지 않을까? 싶지만, 그렇지 않다. 

`Integer`는 `Number`클래스의 하위 클래스가 맞지만, `Box<Integer>`는 `Box<Number>`의 하위 클래스가 아니기 때문이다.



![diagram showing that Box<Integer> is not a subtype of Box<Number>](https://docs.oracle.com/javase/tutorial/figures/java/generics-subtypeRelationship.gif)



둘의 공통된 상위 클래스는 `Object`뿐이다.

<br><br>

### Generic Classes and Subtyping

제네릭 클래스의 하위 클래스를 만들기 위해서는 `extends`, `implements`를 사용하면 된다. 

예를 들어 `Collection` 인터페이스는 `List`인터페이스가 상속했으며, `List`인터페이스는 `ArrayList`가 구현했다. 따라서 아래와 같이 표현된다.



![diagram showing a sample collections hierarchy: ArrayList<String> is a subtype of List<String>, which is a subtype of Collection<String>.](https://docs.oracle.com/javase/tutorial/figures/java/generics-sampleHierarchy.gif)



`List`를 상속한 `PayloadList`를 정의하면 다음과 같다. 

```java
interface PayloadList<E,P> extends List<E> {
  void setPayload(int index, P val);
  ...
}
```

type P에 따라 상속 관계는 다음과 같이 표현된다.

- `PayloadList<String,String>`
- `PayloadList<String,Integer>`
- `PayloadList<String,Exception>`

![diagram showing an example PayLoadList hierarchy: PayloadList<String, String> is a subtype of List<String>, which is a subtype of Collection<String>. At the same level of PayloadList<String,String> is PayloadList<String, Integer> and PayloadList<String, Exceptions>.](https://docs.oracle.com/javase/tutorial/figures/java/generics-payloadListHierarchy.gif)



<br><br>

### Type Inference 타입 추론

타입 추론이란 자바 컴파일러가 메서드 호출과 정의를 보고 타입 인자를 결정하는 것이다. 추론 알고리즘은 타입들의 인자와 반환 결과도 결정한다.

추론 알고리즘은 **모든 인자에 대해 작동하는 가장 구체적인 타입**을 찾으려고 한다.

<br>

두 개의 인자에 대해 적용되는 타입은 `Serializable`이다.

```java
static <T> T pick(T a1, T a2) { return a2; }
Serializable s = pick("d", new ArrayList<String>());
```

<br>

#### Type Inference and Generic Methods

위에서도 언급했듯이, 타입 추론은 제네릭 메서드를 일반 메서드처럼 사용할 수 있도록 해준다. 

예시를 보자.

```java
public class BoxDemo {

  public static <U> void addBox(U u, List<Box<U>> boxes) {
    Box<U> box = new Box<>();
    box.set(u);
    boxes.add(box);
  }

  public static <U> void outputBoxes(List<Box<U>> boxes) {
    int counter = 0;
    for (Box<U> box: boxes) {
      U boxContents = box.get();
      System.out.println("Box #" + counter + " contains [" + boxContents.toString() + "]");
      counter++;
    }
  }

  public static void main(String[] args) {
	ArrayList<Box<Integer>> listOfIntegerBoxes = new ArrayList<>();
    BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);	// 타입 명시
    BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);			// 타입 생략
    BoxDemo.addBox(Integer.valueOf(30), listOfIntegerBoxes);
    BoxDemo.outputBoxes(listOfIntegerBoxes);
  }
}
```

결과는 다음과 같다.

```java
Box #0 contains [10]
Box #1 contains [20]
Box #2 contains [30]
```

원래는 다음과 같이 타입을 명시해준다.

```java
BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
```

타입을 생략해도 컴파일러가 타입을 추론한다.

```java
BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
```

<br>

#### Type Inference and Instantiation of Generic Classes

객체 생성할 때도 타입 추론이 사용된다.

```java
Map<String, List<String>> myMap = new HashMap<String, List<String>>();
```

역시 생략 가능하다.

```java
Map<String, List<String>> myMap = new HashMap<>();
```

타입 추론을 사용할 때는 `다이아몬드(<>)`가 있어야 한다. 없다면 이는 raw type이 되어 경고가 발생한다.

```java
Map<String, List<String>> myMap = new HashMap(); // unchecked conversion warning
```

<br>

#### Target Types

자바 컴파일러는 제네릭 메서드 호출할 때, `target typing`을 이용해 타입 매개변수를 추론한다. 

*target type*이란 표현식이 있는 곳에 따라 자바 컴파일러가 기대하는 데이터 타입을 뜻한다. 

 `Collections.emptyList` 메서드는 다음과 같다.

```java
static <T> List<T> emptyList();
```

할당할 때는 다음과 같이 사용되는데, 여기서는 `List<String>`의 객체를 기대한다. 이것이 target type이다. 

```java
List<String> listOne = Collections.emptyList();
```

`emptyList`는 `List<T>`를 반환하기에 컴파일러는 타입 인자 T가 `String`일 것이라고 추론한다. 

Java7 이전에는 다음과 같이 타입을 명시해야 했다. 

```java
List<String> listOne = Collections.<String>emptyList();
```

다음과 같은 메서드에서 사용할 때는 명시가 필요했다. 

```java
void processStringList(List<String> stringList) {
    // process stringList
}
```

만약 이렇게 호출한다면 자바 7에서는 에러 메세지가 발생한다.

```java
processStringList(Collections.emptyList());
```

```java
List<Object> cannot be converted to List<String>
```

따라서 다음과 같이 명시해야 했다.

```java
processStringList(Collections.<String>emptyList());
```

자바 8부터는 컴파일러가 이마저도 추론해서 다음과 같이 사용할 수 있다. 

```java
processStringList(Collections.emptyList());
```

<br><br>

### Wildcards

제네릭 코드에서 물음표 표시 `?`는 와일드 카드라고 불리며 이는 unknown type을 뜻한다. 

와일드카드는 타입 파라미터, 필드, 지역 변수 등에 다양한 곳에 사용된다. 하지만 타입 인자로는 사용되지 않는다.

<br><br>

### Upper Bounded Wildcards

와일드카드도 upper bound를 통해 변수에 대한 제한을 풀어줄 수 있다. 

만약 `List<Integer>`, `List<Double>`, *and* `List<Number>`에 대해 작동하는 메서드를 만들고 싶다면 와일드 카드를 사용하면 된다. 

와일드카드는 `?`를 사용해 `List<? extends Number>`와 같이 선언한다.

이는 단순히 `List<Number>`라고 정의한 메서드보다 덜 제한적이다.

다음 메서드를 보자.

```java
public static void process(List<? extends Foo> list) { /* ... */ }
```

`Foo` 혹은 하위 타입을 매개변수로 하며, `process` 메서드는 `Foo`타입으로 원소에 접근한다. 

```java
public static void process(List<? extends Foo> list) {
    for (Foo elem : list) {
        // ...
    }
}
```

`sumOfList` 메서드는 `Number` 타입으로 원소의 합을 계산한다.

```java
public static double sumOfList(List<? extends Number> list) {
    double s = 0.0;
    for (Number n : list)
        s += n.doubleValue();
    return s;
}
```

다음 코드는 `Integer`를 명시하고 6.0이라는 결과를 반환한다.

```java
List<Integer> li = Arrays.asList(1, 2, 3);
System.out.println("sum = " + sumOfList(li));
```

`Double`도 똑같이 적용될 수 있다. 

```java
List<Double> ld = Arrays.asList(1.2, 2.3, 3.5);
System.out.println("sum = " + sumOfList(ld));
```

<br><br>

### Unbounded Wildcards

unbounded wildcard type은 와일드카드 문자를 사용하여 표시한다. (ex. `List<?>`)

unbounded wildcard는 2가지의 경우에 사용한다.

- `Object` 클래스가 제공하는 메서드를 사용할 경우

- 타입 매개변수와 상관없는 메서드를 사용할 경우, 

  - 예를 들어 `List.size`, `List.clear` 등
  - `Class<T>` 가 `T`에 의존하지 않는다면 `Class<?>`가 사용된다.

  

예시를 보자. `printList`는 모든 타입의 리스트를 출력하기 원한다.

```java
public static void printList(List<Object> list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}
```

그러나 위의 경우 `List<Integer>`, `List<String>`, `List<Double>` 등을 출력하지 못한다. 왜냐하면 이는 `List<Object>`의 하위 타입이 아니기 때문이다. 

따라서 와일드카드를 이용해 바꿔보자.

```java
public static void printList(List<?> list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
}
```

어떤 타입이든 이는 `List<?>`의 하위 타입이므로 사용 가능하다. 

```java
List<Integer> li = Arrays.asList(1, 2, 3);
List<String>  ls = Arrays.asList("one", "two", "three");
printList(li);
printList(ls);
```

<br><br>

### Lower Bounded Wildcards

upper bounded wildcard와 반대로 lower bounded wildcard도 있다. 

이는 와일드카드 문자 뒤에 `super`키워드를 사용한다. `<? super A>`.

> 참고
>
> upper bound / lower bound 중 하나만 가능하다.



다음과 같이 정의하면 `List<Integer>`, `List<Number>`,`List<Object>`모두 사용할 수 있다.

```java
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 10; i++) {
        list.add(i);
    }
}
```

<br><br>

### Wildcards and Subtyping

제네릭과 같이 와일드카드에도 하위 타입을 사용할 수 있다.

<br>

`List<Integer>` and `List<Number>`의 공통된 상위 클래스는 `List<?>`이다.

![diagram showing that the common parent of List<Number> and List<Integer> is the list of unknown type](https://docs.oracle.com/javase/tutorial/figures/java/generics-listParent.gif)



```java
List<? extends Integer> intList = new ArrayList<>();
List<? extends Number>  numList = intList;  // OK. List<? extends Integer> is a subtype of List<? extends Number>
```

`Integer`는 `Number`의 하위 타입이고 `numList`은 `Number` 객체의 리스트다.  따라서 `intList`와 `numList`의 관계는 아래처럼 표현할 수 있다. 

![diagram showing that List<Integer> is a subtype of both List<? extends Integer> and List<?super Integer>. List<? extends Integer> is a subtype of List<? extends Number> which is a subtype of List<?>. List<Number> is a subtype of List<? super Number> and List>? extends Number>. List<? super Number> is a subtype of List<? super Integer> which is a subtype of List<?>.](https://docs.oracle.com/javase/tutorial/figures/java/generics-wildcardSubtyping.gif)



<br><br>

### Wildcard Capture and Helper Methods

In some cases, the compiler infers the type of a wildcard. For example, a list may be defined as `List<?>` but, when evaluating an expression, the compiler infers a particular type from the code. This scenario is known as *wildcard capture*.

컴파일러는 와일드카드의 타입을 추론한다. 이를 *wildcard capture*라고 한다. 

그런데 어떨 때는 `capture of`라는 에러 메세지를 뜨면서 타입 추론이 제대로 되지 않는다. 

어떤 경우인지 예시로 보자.

```java
import java.util.List;

public class WildcardError {

    void foo(List<?> i) {
        i.set(0, i.get(0));
    }
}
```

위의 메서드에서는 `List<?> i`를 타입 매개변수로 받는다. 따라서 컴파일러는 이를 `Object`타입으로 여긴다. 

그런데 `i.set(0,i,get(0))`이 부분에서 문제가 생긴다. 

`List.set(int index, E elem)`는 호출할 때 `E`를 명시해줘야 하는데, 컴파일러는 이를 제대로 알지 못한다. 와일드카드를 쓴다는 건 타입에 의존하지 않기 위함이기 때문이다. 

따라서 type safe하지 않으므로 컴파일러가 다음과 같은 에러를 발생한다.

```java
WildcardError.java:6: error: method set in interface List<E> cannot be applied to given types;
    i.set(0, i.get(0));
     ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
1 error
```

`set`메서드에서 기대하는 값은 int, CAP#1이다. 그런데 실제 값은 다르다. 

그러면 어떻게 해야할까? 

이를 해결하기 위해 `helper method`라는 것을 정의해주면 된다. 굉장히 간단하다.

```java
public class WildcardFixed {

    void foo(List<?> i) {
        fooHelper(i);
    }


    // Helper method created so that the wildcard can be captured
    // through type inference.
    private <T> void fooHelper(List<T> l) {
        l.set(0, l.get(0));
    }

}
```

아래처럼 타입 매개변수를 받는 제네릭 메서드를 선언해주면 된다. 

컴파일러는 `T`를 확인하고 이에 따라 동작을 할 수 있다. 

이름은 다음과 같이 짓는다.  `*originalMethodName*Helper`.

<br>

좀 더 어려운 예시를 보자.

```java
import java.util.List;

public class WildcardErrorBad {

    void swapFirst(List<? extends Number> l1, List<? extends Number> l2) {
      Number temp = l1.get(0);
      l1.set(0, l2.get(0)); // expected a CAP#1 extends Number,
                            // got a CAP#2 extends Number;
                            // same bound, but different types
      l2.set(0, temp);	    // expected a CAP#1 extends Number,
                            // got a Number
    }
}
```

다음과 같이 unsafe하게 메서드를 호출해보자.

```java
List<Integer> li = Arrays.asList(1, 2, 3);
List<Double>  ld = Arrays.asList(10.10, 20.20, 30.30);
swapFirst(li, ld);
```

두 개의 리스트 모두 `List<? extends Number>`는 만족하지만, 첫 번째 값을 바꿀 수 있을까?

다음과 같은 에러를 발생시킨다.

```java
WildcardErrorBad.java:7: error: method set in interface List<E> cannot be applied to given types;
      l1.set(0, l2.get(0)); // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:10: error: method set in interface List<E> cannot be applied to given types;
      l2.set(0, temp);      // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:15: error: method set in interface List<E> cannot be applied to given types;
        i.set(0, i.get(0));
         ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
3 errors
```

이는 helpermethod을 생성할 수 없다. `Integer`와 `Double`를 바꿀 수 없기 때문이다.

<br><br>

### Guidelines for Wildcard Use

제네릭을 공부하면서 가장 헷갈리는 부분 중 하나는 언제 upper bounded wildcard를 쓰고, lower bounded wildcard를 쓰냐이다. 

가이드 라인을 제시한다.

이전에 변수를 다음과 같이 생각해보자.

- **An "In" Variable**

  "in" 변수는 code에게 data를 전달하는 역할을 한다. 예를 들어 복사 메서드는 두 개의 변수를 사용한다.  `copy(src, dest)`.  `src`변수는 복사할 데이터를 제공한다. 따라서 이를 `in` 매개변수라 한다. 

- **An "Out" Variable**

  "out"변수는 데이터를 다른 곳에서 사용할 수 있도록 보관한다. `copy(src, dest)`메서드에서 `dest`변수는 값을 저장한다. 따라서 `out` 매개변수다.

물론 하나의 변수가 둘 다 쓰이는 경우도 있다.

<br>

**Wildcard Guidelines:**

- `in` 변수는 `extends` 키워드를 통해 upper bounded wildcard로 정의하라.
- `in` 변수는 `super` 키워드를 통해 lower bounded wildcard로 정의하라.
- `in`변수를 `Object`클래스에 정의된 메서드로 사용한다면 unbounded wildcard를 사용하라.
- 코드가 변수를 `in` ,`out` 변수로 접근해야 한다면 와일드카드를 사용하지 마라.

- 와일드카드를 메서드 반환 타입으로 사용하지 마라. 

<br><br>

### Type Erasure

제네릭을 사용하기 위해 자바 컴파일러는 type erasure를 사용한다. 

- 모든 타입 매개변수를 bounds 혹은 `Object`로 대체한다. 따라서 바이트 코드는 기존의 클래스, 인터페이스, 메소드만 포함한다.
- type safety를 지키기 위해 필요하다면 type cast가 들어간다.
- 다형성을 위해 bridge methods를 생성한다.

type erasure를 사용하는 이유는 제네릭 이전의 코드와의 호환성을 위해서이다. 

Type erasure는 매개변수 타입을 위해 새로운 클래스가 생성되지 않음을 보장해준다. 따라서 제네릭은 런타임 오버헤드가 없다.

<br><br>

### Erasure of Generic Types

 type erasure 프로세스동안 자바 컴파일러는 모든 타입 파라미터를 지운다. 그리고 type이 bounded라면 첫 번째 bound로, unbounded라면 `Object`타입으로 대체한다.

제네릭으로 구현한 단일 연결리스트의 예시를 보자.

```java
public class Node<T> {

    private T data;
    private Node<T> next;

    public Node(T data, Node<T> next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}
```

`T`는 unbounded이므로 `Object`로 변경된다.

```java
public class Node {

    private Object data;
    private Node next;

    public Node(Object data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Object getData() { return data; }
    // ...
}
```

이 경우 `Node`는 `Comparable<T>`로 bounded 되고 있다.

```java
public class Node<T extends Comparable<T>> {

    private T data;
    private Node<T> next;

    public Node(T data, Node<T> next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}
```

따라서 첫 번째 bounded 클래스인 `Comparable`로 대체된다.

```java
public class Node {

    private Comparable data;
    private Node next;

    public Node(Comparable data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Comparable getData() { return data; }
    // ...
}
```

<br><br>

### Erasure of Generic Methods

컴파일러는 메서드 인자에서의 타입 매개변수도 지운다. 

```java
// Counts the number of occurrences of elem in anArray.
//
public static <T> int count(T[] anArray, T elem) {
    int cnt = 0;
    for (T e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}
```

unbounded 이므로 Object로 대체된다.

```java
public static int count(Object[] anArray, Object elem) {
    int cnt = 0;
    for (Object e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}
```

클래스와 메서드를 다음과 같이 가정하자.

```java
class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }
```

```java
public static <T extends Shape> void draw(T shape) { /* ... */ }
```

컴파일러는 first bound로 대체한다.

```java
public static void draw(Shape shape) { /* ... */ }
```

<br><br>

### Effects of Type Erasure and Bridge Methods

type erasure은 기대하지 못한 상황을 발생시킨다. 

아래 예시는 기대하지 못한 상황이 어떻게 일어나는지를 보여준다.

```java
public class Node<T> {

    public T data;

    public Node(T data) { this.data = data; }

    public void setData(T data) {
        System.out.println("Node.setData");
        this.data = data;
    }
}

public class MyNode extends Node<Integer> {
    public MyNode(Integer data) { super(data); }

    public void setData(Integer data) {
        System.out.println("MyNode.setData");
        super.setData(data);
    }
}
```

```java
MyNode mn = new MyNode(5);
Node n = mn;            // A raw type - compiler throws an unchecked warning
n.setData("Hello");     // Causes a ClassCastException to be thrown.
Integer x = mn.data;    
```

type erasure를 거치면 다음과 같이 변환된다. 

```java
MyNode mn = new MyNode(5);
Node n = (MyNode)mn;         // A raw type - compiler throws an unchecked warning
n.setData("Hello");          // Causes a ClassCastException to be thrown.
Integer x = (String)mn.data; 
```

왜 `n.setData("Hello")`가 문제가 될까? Bridge method가 이유다.

<br><br>

### Bridge Methods

제네릭 클래스, 인터페이스를 상속한 클래스를 컴파일할 때, 컴파일러는 합성 메서드를 만들어 사용한다. 이를 bridge method라 한다. 

type erasure 이후, `Node`와 `MyNode` 클래스는 다음과 같이 변한다.

```java
public class Node {

    public Object data;

    public Node(Object data) { this.data = data; }

    public void setData(Object data) {
        System.out.println("Node.setData");
        this.data = data;
    }
}

public class MyNode extends Node {

    public MyNode(Integer data) { super(data); }

    public void setData(Integer data) {
        System.out.println("MyNode.setData");
        super.setData(data);
    }
}
```

type erasure 이후 메서드 시그니처가 맞지 않는다.

 `Node.setData(T)` 메서드는  `Node.setData(Object)`가 된다. 

따라서 `MyNode.setData(Integer)`메서드는 `Node.setData(Object)`메서드를 override하지 않게 되어버린다.

type erasure 이후에도 다형성을 유지하기 위해 자바 컴파일러는 원래처럼 동작하도록 bridge method를 만든다.

```java
class MyNode extends Node {

    // Bridge method generated by the compiler
    //
    public void setData(Object data) {
        setData((Integer) data);
    }

    public void setData(Integer data) {
        System.out.println("MyNode.setData");
        super.setData(data);
    }

    // ...
}
```

`MyNode.setData(object)`은 기존의 `MyNode.setData(Integer)` 메서드를 대신해준다.

따라서 이전에 `n.setData("Hello");` 구문에서 `"Hello"`는 `Integer`로 casting이 안되기 때문에 예외가 발생한다.

<br><br>

### Restrictions on Generics

To use Java generics effectively, you must consider the following restrictions:



#### Cannot Instantiate Generic Types with Primitive Types

Consider the following parameterized type:

```java
class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    // ...
}
```

When creating a `Pair` object, you cannot substitute a primitive type for the type parameter `K` or `V`:

```java
Pair<int, char> p = new Pair<>(8, 'a');  // compile-time error
```

You can substitute only non-primitive types for the type parameters `K` and `V`:

```java
Pair<Integer, Character> p = new Pair<>(8, 'a');
```

Note that the Java compiler autoboxes `8` to `Integer.valueOf(8)` and '`a`' to `Character('a')`:

```java
Pair<Integer, Character> p = new Pair<>(Integer.valueOf(8), new Character('a'));
```



#### Cannot Create Instances of Type Parameters

You cannot create an instance of a type parameter. For example, the following code causes a compile-time error:

```java
public static <E> void append(List<E> list) {
    E elem = new E();  // compile-time error
    list.add(elem);
}
```

As a workaround, you can create an object of a type parameter through reflection:

```java
public static <E> void append(List<E> list, Class<E> cls) throws Exception {
    E elem = cls.newInstance();   // OK
    list.add(elem);
}
```

You can invoke the `append` method as follows:

```java
List<String> ls = new ArrayList<>();
append(ls, String.class);
```

#### Cannot Declare Static Fields Whose Types are Type Parameters

A class's static field is a class-level variable shared by all non-static objects of the class. Hence, static fields of type parameters are not allowed. Consider the following class:

```java
public class MobileDevice<T> {
    private static T os;

    // ...
}
```

If static fields of type parameters were allowed, then the following code would be confused:

```java
MobileDevice<Smartphone> phone = new MobileDevice<>();
MobileDevice<Pager> pager = new MobileDevice<>();
MobileDevice<TabletPC> pc = new MobileDevice<>();
```

Because the static field `os` is shared by `phone`, `pager`, and `pc`, what is the actual type of `os`? It cannot be `Smartphone`, `Pager`, and `TabletPC` at the same time. You cannot, therefore, create static fields of type parameters.



#### Cannot Use Casts or `instanceof` with Parameterized Types

Because the Java compiler erases all type parameters in generic code, you cannot verify which parameterized type for a generic type is being used at runtime:

```java
public static <E> void rtti(List<E> list) {
    if (list instanceof ArrayList<Integer>) {  // compile-time error
        // ...
    }
}
```

The set of parameterized types passed to the `rtti` method is:

```java
S = { ArrayList<Integer>, ArrayList<String> LinkedList<Character>, ... }
```

The runtime does not keep track of type parameters, so it cannot tell the difference between an `ArrayList<Integer>` and an `ArrayList<String>`. The most you can do is to use an unbounded wildcard to verify that the list is an `ArrayList`:

```java
public static void rtti(List<?> list) {
    if (list instanceof ArrayList<?>) {  // OK; instanceof requires a reifiable type
        // ...
    }
}
```

Typically, you cannot cast to a parameterized type unless it is parameterized by unbounded wildcards. For example:

```java
List<Integer> li = new ArrayList<>();
List<Number>  ln = (List<Number>) li;  // compile-time error
```

However, in some cases the compiler knows that a type parameter is always valid and allows the cast. For example:

```java
List<String> l1 = ...;
ArrayList<String> l2 = (ArrayList<String>)l1;  // OK
```



#### Cannot Create Arrays of Parameterized Types

You cannot create arrays of parameterized types. For example, the following code does not compile:

```java
List<Integer>[] arrayOfLists = new List<Integer>[2];  // compile-time error
```

The following code illustrates what happens when different types are inserted into an array:

```java
Object[] strings = new String[2];
strings[0] = "hi";   // OK
strings[1] = 100;    // An ArrayStoreException is thrown.
```

If you try the same thing with a generic list, there would be a problem:

```java
Object[] stringLists = new List<String>[2];  // compiler error, but pretend it's allowed
stringLists[0] = new ArrayList<String>();   // OK
stringLists[1] = new ArrayList<Integer>();  // An ArrayStoreException should be thrown,
                                            // but the runtime can't detect it.
```

If arrays of parameterized lists were allowed, the previous code would fail to throw the desired `ArrayStoreException`.



#### Cannot Create, Catch, or Throw Objects of Parameterized Types

A generic class cannot extend the `Throwable` class directly or indirectly. For example, the following classes will not compile:

```java
// Extends Throwable indirectly
class MathException<T> extends Exception { /* ... */ }    // compile-time error

// Extends Throwable directly
class QueueFullException<T> extends Throwable { /* ... */ // compile-time error
```

A method cannot catch an instance of a type parameter:

```java
public static <T extends Exception, J> void execute(List<J> jobs) {
    try {
        for (J job : jobs)
            // ...
    } catch (T e) {   // compile-time error
        // ...
    }
}
```

You can, however, use a type parameter in a `throws` clause:

```java
class Parser<T extends Exception> {
    public void parse(File file) throws T {     // OK
        // ...
    }
}
```



#### Cannot Overload a Method Where the Formal Parameter Types of Each Overload Erase to the Same Raw Type

A class cannot have two overloaded methods that will have the same signature after type erasure.

```java
public class Example {
    public void print(Set<String> strSet) { }
    public void print(Set<Integer> intSet) { }
}
```

The overloads would all share the same classfile representation and will generate a compile-time error.

<br>

<br>

### REFERENCE

[java-tutorials : generics](https://docs.oracle.com/javase/tutorial/java/generics/index.html)