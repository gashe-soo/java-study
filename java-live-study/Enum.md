# Enum

### Enum?

Enum은 참조 타입으로 몇 개의 열거 상수 중에서 하나의 상수를 저장하는 데이터 타입이다. 

이전에는 상수를 관리하기 위해 다음과 같이 사용했다.

```java
class Week{
    static final int MONDAY = 1;
    static final int TUESDAY = 2;
    static final int WEDNESDAY = 3;
    static final int THURSDAY = 4;
    static final int FRIDAY = 5;
}

class Card{
    static final int SPACE = 1;
    static final int DIAMOND = 2;
    static final int HEART = 3;
    static final int CLOVER = 4;
}
```

그런데 문제점은 타입이 안전(Typesafety)하지 않다는 거다. 

무슨 말인가하면

```java
Week.MONDAY == CARD.SPACE
```

위의 식이 성립한다는 것이다. 당연히 상수니까 성립한다. 그러나 우리가 원하는 행동은 아니다. 따라서 원치 않는 결과가 발생했다. 

이를 해결하기 위해 열거 타입을 사용한다.

다음과 같이 선언하여 사용한다.

```java
enum 열거타입이름{}
```

열거 상수는 대문자로 작성한다.

```java
enum Week{
    MONDAY, TUESDAY, WEDNESDAY,....
}
```

만약 여러 단어로 구성된다면 _(underscore)를 이용하자.

```java
enum Result{
    LOGIN_SUCCESS, LOGIN_FAILED
}
```

<br>

열거 타입도 데이터 타입이므로 변수를 선언하고 사용해야 한다.

```java
열거타입 변수;
Week today = Week.SUNDAY;
Week bday = null;
```

<br>

처음에 열거 상수는 참조 타입이라고 했는데, 이는 열거 객체로 생성된다. 

로딩되면서 `Week`열거 타입은 메소드 영역에 올라가게 된다. 또한, Decompile해보니 기본적으로 `private`생성자를 가지고 있는데,  이를 통해 싱글톤으로 생성되는 것을 알 수 있다.

```java
public enum Week {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY;

    private Week() {
    }
}
```

<br>

`enum`의 경우 메소드와 필드 또한 지정할 수 있다. 다만 그럴 경우 열거형 상수에 값을 부여해야 한다.

```java
public enum Week {
    MONDAY(10), TUESDAY(20), WEDNESDAY(30), THURSDAY(40), FRIDAY(50), SATURDAY(60), SUNDAY(70);

    private int val;
    Week(int val) {
        this.val =val;
    }
}
```

<br>

<br>

### Enum 메소드

열거 객체는 여러 메소드를 제공한다.

<br>

#### name()

열거 객체가 가지고 있는 문자열을 리턴한다. 

```java
Week today = Week.THURSDAY;
String name = today.name();
System.out.println("name = " + name);	// name = THURSDAY
```

<br>

#### ordinal()

`ordinal()`메소드는 열거 객체 중 몇 번째 열거 객체인지 알려준다. 순번은 타입을 정의할 때 주어진 순번이다. 이는 0번부터 시작한다. 

```java
Week today = Week.THURSDAY;
int sequence = today.ordinal();
System.out.println("sequence = " + sequence); // sequence = 3
```

다만, `ordinal()`은 유용하지 않다. 

만약에 열거 상수를 맨 앞에 추가한다면 이전에 사용하던 index는 모두 1이 추가되어야 하는데, 그렇다면 이를 사용하던 모든 코드에서 변경이 일어나야 하기 때문이다.

<br>

#### compareTo()

매개변수로 주어진 열거 객체를 기준으로 전후로 몇 번째 위치하는 지 비교한다. 

객체가 매개값의 열거 객체보다 빠르다면 음수, 늦으면 양수가 나온다.

```java
Week yesterday = Week.WEDNESDAY;
Week today = Week.THURSDAY;

int ret1 = today.compareTo(yesterday);
int ret2 = yesterday.compareTo(today);

System.out.println("ret1 = " + ret1);	// ret1 = 1
System.out.println("ret2 = " + ret2);	// ret2 = -1
```

<br>

#### valueOf()

매개값으로 주어지는 문자열과 동일한 문자열을 가지는 열거 객체를 리턴한다. 

```java
Week today = Week.valueOf("THURSDAY");
System.out.println("today = " + today.name());	// today = THURSDAY

// error
Week notDay = Week.valueOf("thursday");		// 소문자는 불가능.
System.out.println("notDay = " + notDay);
```

<br>

#### values()

열거 타입의 모든 열거 객체들을 배열로 만들어 리턴한다. 

```java
Week[] days = Week.values();
for(Week day : days)
    System.out.println(day);

// 결과
MONDAY
TUESDAY
WEDNESDAY
THURSDAY
FRIDAY
SATURDAY
SUNDAY
```

<br><br>

### java.lang.Enum

열거 타입은 컴파일시 `java.lang.Enum`클래스를 상속받는다. 

그래서 `Enum`클래스에서 정의한 메소드를 사용할 수 있는 것이다. 

다만 `values()`의 경우, 컴파일러가 컴파일 시에 자동으로 추가해준다. 

<br>

<br>

### EnumSet

>  A specialized Set implementation for use with enum types. All of the elements in an enum set must come from a single enum type that is specified, explicitly or implicitly, when the set is created. 
> Enum sets are represented internally as bit vectors. This representation is extremely compact and efficient. The space and time performance of this class should be good enough to allow its use as a high-quality, typesafe alternative to traditional int-based "bit flags." Even bulk operations (such as containsAll and retainAll) should run very quickly if their argument is also an enum set.

이름 그대로 `Enum`클래스를 위한 `Set`이다. 

`Set`를 생성할 때 하나의 열거 타입이 원소가 되어야 한다. 

```java
 // 모든 원소
EnumSet<Week> enumSet = EnumSet.allOf(Week.class);

// 특정 원소
EnumSet<Week> enumSet2 = EnumSet.of(Week.MONDAY, Week.TUESDAY);

// 제외 원소
EnumSet<Week> enumSet3 = EnumSet.complementOf(enumSet2);

// 범위
EnumSet<Week> enumSet4 = EnumSet.range(Week.MONDAY, Week.FRIDAY);

System.out.println("enumSet = " + enumSet);
System.out.println("enumSet2 = " + enumSet2);
System.out.println("enumSet3 = " + enumSet3);
System.out.println("enumSet4 = " + enumSet4);

// result
enumSet = [MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY]
enumSet2 = [MONDAY, TUESDAY]
enumSet3 = [WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY]
enumSet4 = [MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY]
```

더 궁금하다면 [여기](https://docs.oracle.com/javase/8/docs/api/)를 읽어보자.

<br>

EnumMap도 EnumSet과 동일하다. 이는 문서를 참고하자.

<br>

<br>

### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어

https://docs.oracle.com/javase/8/docs/api/