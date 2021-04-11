# Item 62. 다른 타입이 적절하다면 문자열 사용을 피하라



문자열(String)은 텍스트를 표현하도록 설계되었다. 그런데 이를 의도하지 않은 용도로도 쓰이는 경향이 있다.

<br>

문자열을 쓰지 않아야 할 사례를 보자.

<br>

### Case 1. 문자열이 아닌 다른 값 타입을 대신하기에 적합하지 않다.

입력으로부터 데이터를 받을 때 주로 문자열을 사용한다. 

하지만, 진짜 문자열일 때만 그렇게 하는 것이 좋다.

수치형이라면 `int`,`float`, `BigInteger` 의 수치 타입으로, 

예/아니오의 질문의 답이면 `boolean` 혹은 `Enum`타입으로 변환한다.

적절한 기본, 참조 타입이 없다면 새로 작성하라

<br>

### Case 2. 열거 타입을 대신하기에 적합하지 않다.

상수를 열거할 때는 문자열보다는 열거 타입이 월등히 낫다.

<br>

### Case 3. 혼합 타입을 대신하기에 적합하지 않다.

여러 요소가 혼합된 데이터를 하나의 문자열로 표현하는 것은 대체로 좋지 않다. 

```java
String compoundKey = className + "#" + i.next();
```

이는 단점이 많다. 

만약 className, i.next()에서 두 요소를 구분해주는 문자`#`이 사용되었다면? 혼란의 시작이다.

또한, 개별로 접근하기 위해서는 문자열을 파싱해야 해서 **느리고, 귀찮고, 오류 가능성도 커진다.**

그리고 `equals`,`toString`, `compareTo` 메서드를 제공할 수 없으며 `String`이 제공하는 기능에만 의존해야 한다.

<br>

### Case 4.  권한을 표현하기에 적합하지 않다.

권한(capacity)을 문자열로 표현하는 경우가 있다. 

예를 들어 스레드 지역변수 기능을 설계한다고 해보자. 이는 각 스레드가 자신만의 변수를 갖게 해주 는 기능이다. (스레드가 실행되는 동안 변수를 사용할 수 있다.)

클라이언트가 제공한 문자열 키로 스레드별 지역변수를 식별하도록 하자.

```java
public class ThreadLocal{
    private ThreadLocal() {}
    
    // 현 스레드의 값을 key로 구분해 저장한다.
    public static void set(String key, Object value);
    
    // (key가 가리키는)현 스레드의 값을 반환한다.
    public static Object get(String key);
}
```

이 방식의 문제는 스레드 구분용 문자열 키가 전역 공간에서 공유된다는 점이다. 

의도대로 동작하기 위해서는 각 클라이언트가 고유한 키를 제공해야 한다.

그런데, 다른 클라이언트가 같은 키를 쓰게 된다면 원하는대로 동작하지 않을 것이다. 

당연히 보안도 취약하다. 해커가 의도적으로 같은 키를 쓴다면 권한을 얻어 스레드의 값을 사용할 수 있다.

<br>

그래서 문자열 대신 위조할 수 없는 키를 사용해야 한다. 

```java
public class ThreadLocal{
    private ThreadLocal(){}
    
    public static class Key{
        Key(){}
    }
    // 위조 불가능한 고유 키를 생성한다.
    public static Key getKey(){
       	return new Key();
    }
    
    public static void set(Key key, Object value);
    public static Object get(Key key);
}
```

앞선 문제를 해결해주지만, 개선 여지가 있다.

`set`, `get`은 정적 메서드일 이유가 없다. 그러므로 `Key`클래스의 인스턴스 메서드로 바꾸자.

그런데 이렇게 하면 `Key`는 더 이상 스레드 지역변수를 구분하기 위한 키가 아니라, 그 자체가 스레드 지역변수가 된다. 

따라서 `Key`의 이름을 ThreadLocal로 바꿔도 무방하다.

```java
public final class ThreadLocal{
    public ThreadLocal();
    public void set(Object value);
    public Object get();
}
```

`get`로 얻은 Object를 실제 타입으로 형변환해 써야 해서 타입 안전하지 않다. 

그러므로 매개변수화 타입으로 선언하여 문제를 해결한다.

```java
public final class ThreadLocal<T>{
    public ThreadLocal();
    public void set(T value);
    public T get();
}
```

<br>

### 요약

더 적합한 데이터 타입이 있거나 새로 작성할 수 있다면 문자열을 쓰지 말자.

문자열은 잘못 사용하면 번거롭고, 덜 유연하고, 느리고, 오류 가능성도 크다.

<br><br>