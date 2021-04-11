# Item 10. equals는 일반 규칙을 지켜 재정의하라

시작하기 전에, 읽으면서도 꽤나 어렵고 이해하기 힘든 내용이었다. 

최대한 쉽게 설명해보겠다. 또한, 읽으면서 생각한 **내 의견은 _italic체_로 표시**해두겠다.

<br>

### 결론

item 10에서는 결론부터 말하고 싶다. 왜냐하면 이 얘기를 계속 언급하기 때문이다. 

> 꼭 필요한 경우가 아니면 equals를 재정의하지 말자.

왜 하지 말라는 걸까? 이유를 알아보자.

<br>

### 재정의하지 않아야 하는 CASE

1. 각 인스턴스가 본질적으로 `고유`하다
   - 값이 아닌 동작하는 개체를 표현하는 클래스가 해당한다.
   - _본질적으로 고유하므로 애초에 equals를 사용할 필요가 없다._

2. 인스턴스의 `논리적 동치성`을 검사할 일이 없다.

   - 인스턴스가 같은 정규표현식을 나타내는지 검사하는 방법도 있지만, 설계자가 필요하지 않다고 판단한다면 재정의할 필요 없다.

3. 상의 클래스에서 재정의한 _equals_가 하위 클래스에도 딱 들어맞는다.

   - `Set` 구현체는 `AbstractSet`이 구현한 equals를 상속받아 쓴다. 
   - `List` 구현체는 `AbstractList`로부터 , `Map` 구현체는`AbstractMap`로부터 상속받아 사용한다.

4. 클래스가 `private` 이거나 `package-private`이고 *equals* 메서드를 호출할 일이 없다. 

   - 다음과 같이 사용한다면 애초에 오류를 회피할 수 있다!

     ```java
     @Override
     public boolean equals(Object o){
         throw new AssertionError();
     }
     ```

   <br>

### 재정의할 때는 언제일까?

객체가 같은 지는 Object의 *equals*가 해준다. (객체 식별성)

그런데도 재정의한다는 것은 `논리적 동치성`을 확인해야 할 때이다. 주로 값 클래스(`Integer`, `String`처럼 값을 표현하는 클래스)가 해당한다. 

다만, 

값이 같은 인스턴스가 2개 이상 만들어지지 않음을 보장하는 인스턴스 통제 클래스라면 재정의가 필요없다. (ex. ENUM)

이는 논리적으로 같은 인스턴스가 2개 이상 없으므로 `객체 식별성` = `논리적 동치성`을 보장해주기 때문이다. 

<br>

### 어떻게 재정의해야할까?

*equals*를 재정의할 때는 반드시 일반 규약을 따라야 한다. 

일반 규약은 Object 명세에 적혀 있다.

```m
The {@code equals} method implements an equivalence relation
 on non-null object references:

- It is reflexive: for any non-null reference value
     {@code x}, {@code x.equals(x)} should return
     {@code true}.
- It is symmetric: for any non-null reference values
     {@code x} and {@code y}, {@code x.equals(y)}
     should return {@code true} if and only if
     {@code y.equals(x)} returns {@code true}.
- It is transitive: for any non-null reference values
     {@code x}, {@code y}, and {@code z}, if
     {@code x.equals(y)} returns {@code true} and
     {@code y.equals(z)} returns {@code true}, then
     {@code x.equals(z)} should return {@code true}.
- It is consistent: for any non-null reference values
     {@code x} and {@code y}, multiple invocations of
     {@code x.equals(y)} consistently return {@code true}
     or consistently return {@code false}, provided no
     information used in {@code equals} comparisons on the
     objects is modified.
- For any non-null reference value {@code x},
     {@code x.equals(null)} should return {@code false}.
 
```

영어니깐 번역을 보자.....ㅎ

>equals 메서드는 동치관계(equivalence relation)를 구현하며, 다음을 만족한다.
>
>- 반사성(reflexivity) : null 아닌 모든 참조 값 x에 대해, x.equals(x)는 true다.
>- 대칭성(symmetry) : null 아닌 모든 참조 값 x,y에 대해, x.equals(y)가 true면 y.equals(x)도 true다.
>- 추이성(transivity) : null 아닌 모든 참조 값 x,y,z에 대해, x.equals(y)가 true이고 y.equals(z)도 true면 x.equals(z)도 true다.
>- 일관성(consistency) : null 아닌 모든 참조 값 x,y에 대해, x.equals(y)를 반복해서 호출하면 항상 같은 값을 반환한다.
>- null - 아님 : null 아닌 모든 참조 값 x에 대해, x.equals(null)은 false다.

<br>

만약 규약을 어기면 프로그램은 이상하게 동작하거나 종료될 것이고, 원인을 찾기도 어려울 것이다. 

예를 들어 `Collection` 클래스들은 *equals* 규약을 지킨다고 가정하고 동작한다. 

<br>

그럼 하나씩 뜯어보며 이해해보자.

<br>

### 동치관계(equivalence relation)

책에선 동치관계를 이렇게 말한다.

> 쉽게 말해, 집합을 서로 같은 원소로 이뤄진 부분집합으로 나누는 연산이다. 이 부분집합을 동치류(동치 클래스)라 한다.

??? 쉽게 말한건가 싶다. 

좀 더 쉽게 말해보면, 같은 애들끼리 모이게 하는 것이다. 이들은 서로가 같으므로 교환해도 상관없게 된다. 

*equals* 메소드가 쓸모있으려면 같은 동치류에 속한 원소끼리는 교환이 가능해야 한다. 

다른 규약을 하나씩 살펴보자.

<br>

### 반사성 (Reflexivity)

객체는 자기 자신과 같아야 한다는 뜻이다. 

당연히 지켜야 하고 지켜지는 요건이다. 

만약 이 요건을 못 지킨다면 인스턴스를 `collection`에 넣어도 `contains`메서드로는 찾지 못할 것이다. (`contains` 메서드는 `equals`로 비교하며 찾는다.)

<br>

### 대칭성 (Symmetry)

두 객체는 서로에 대한 동치 여부에 똑같이 답해야 한다는 뜻이다. 

예를 통해 이해하면 빠르다. 

```java
public final class CaseInsensitiveString {
    private final String s;

    public CaseInsensitiveString(String s) {
        //requrieNonNull은 null 체크 기능.
        this.s = Objects.requireNonNull(s);
    }

    @Override
    public boolean equals(Object o) {
        if(o instanceof CaseInsensitiveString)
            return s.equalsIgnoreCase(((CaseInsensitiveString) o).s);
        // 대칭성 위반!
        if(o instanceof String)
            return s.equalsIgnoreCase((String) o);
        return false;
    }
}
```

`CaseInsensitiveString` 클래스는 대소문자를 구별하지 않는다. 

그래서 *equals*에서 

1. 비교 클래스가 같으면 멤버 변수인 s와 비교한다.
2. 비교 클래스가 같지 않지만 String이라면 o의 값과 멤버 변수 s를 비교한다.
3. 1,2에 해당하지 않으면 같지 않다고 판단한다.

슬쩍 보면 뭐가 문제지 싶다. 

하지만 이는 대칭성을 위반했다. 

```java
CaseInsensitiveString cis = new CaseInsensitiveString("Polish");
String s = "polish";
```

이 경우 `cis.equals(s)`는 `true`를 반환한다. `CaseInsensitveString` 클래스는 대소문자를 구별하지 않도록 *equals*를 재정의했기 때문이다. 

그러나 String은 그렇지 않다. 따라서 `s.equals(cis)`는 `false`를 반환할 것이다. 그러므로 대칭성을 위반한다.

```java
List<CaseInsensitiveString> list = new ArrayList<>();
list.add(cis);

// if
list.contains(s);
```

어떤 결과가 나올까? 아마 `false`를 반환할 것이다. (아마인 이유는 jdk마다 다를 수 있기 때문이다.)

이처럼 **equals 규약을 어기면 객체를 사용하는 다른 객체들이 어떻게 반응할 지 알 수 없다.**

<br>

이 문제를 해결하려면 *equals*를 String에도 연동하겠다는 허황한 꿈을 버려야 한다...!

다음처럼 간단한 모습으로 바뀐다.

```java
@Override
    public boolean equals(Object o) {
        return o instanceof CaseInsensitiveString && ((CaseInsensitiveString) o).s.equalsIgnoreCase(s);
    }
```

<br>

### 추이성 (Transivity)

흔히 말하는 삼단논법이다. 

> a = b가 참이고 b = c가 참이면, a = c도 참이다.

이 요건은 간단하지만 자칫하면 어기기 쉽다. 

책에서 드는 예시를 함께 보자. 이 경우가 가장 어려우니 집중해보자. 

<br/>

**상위 클래스에는 없는 새로운 필드를 하위 클래스에 추가**하는 상황이다. *equals*에도 영향을 줄 것이다.

2차원 점을 표현하는 클래스이다.

```java
public class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Point))
            return false;
        Point p = (Point) obj;
        return p.x == x && p.y == y;
    }
}
```

이 클래스를 확장해서 색상 필드를 추가해보자.

```java
public class ColorPoint extends Point {
    private final Color color;

    public ColorPoint(int x, int y, Color color) {
        super(x, y);
        this.color = color;
    }
}
```

`ColorPoint`의 *equals*는 어떻게 재정의해야 할까?

`Point`클래스의 *equals*를 그대로 사용한다면 색상을 비교할 수 없게 된다.

일단 위치와 색상을 비교하는 *equals*를 생각해보자.

```java
// ColorPoint의 equals
@Override
public boolean equals(Object obj) {
    if (!(obj instanceof ColorPoint))
        return false;
    return super.equals(obj) && ((ColorPoint) obj).color == color;
}
```

`ColorPoint` 객체만 비교하며, x,y는 `Point`클래스의 *equals*로 비교하고 색상은 따로 비교한다. 

그럼 이것이 제대로 동작할까?

```java
Point p = new Point(1,2);
ColorPoint cp = new ColorPoint(1,2, Color.RED);

// 다음을 실행하면 어떤 결과가 나올까?
p.equals(cp);	// true, Point클래스의 equals는 x,y만 비교하기 때문이다.
cp.equals(p);	// false, p는 ColorPoint가 아니므로 false를 반환한다. 
```

보다시피 이는 대칭성을 위반한다. 

그렇다면 `ColorPoint`와 `Point`를 비교할 때는 색상을 무시하도록 해보자. 그러면 위의 문제는 해결될 것이다. 

```java
@Override
public boolean equals(Object obj) {
    if(!(obj instanceof Point))
        return false;
    
    // obj가 일반 Point면 색상 무시하고 비교한다.
    if(!(obj instanceof ColorPoint))
        return obj.equals(this);
    
    return super.equals(obj) && ((ColorPoint) obj).color == color;
}
```

이렇게 되면

```java
Point p = new Point(1,2);
ColorPoint cp = new ColorPoint(1,2, Color.RED);
```

이 경우는 대칭성을 만족한다. 그렇다면 이런 경우는 만족할까?

```java
ColorPoint p1 = new ColorPoint(1,2, Color.RED);
Point p2 = new Point(1,2);
ColorPoint p3 = new ColorPoint(1,2, Color.BLUE);

// 어떤 결과가 나올까?
p1.equals(p2);	// true
p2.equals(p3);	// true
p3.equals(p1);	// false
```

이는 추이성을 위반한다.

심지어는 무한 재귀에 빠질 수도 있다.

`Point`의 또 다른 하위 클래스 `SmellPoint`를 만들고 *equals*도 같은 방식으로 구현했다고 하자.

만약 

```java
myColorPoint.equals(mySmellPoint);
```

호출한다면 결과는 StackOverflowError를 일으킨다.

> 🔎왜 StackOverflow인가요?
>
> *equals* 메서드의 구현을 보면
>
> 첫 번째로 `Point`클래스인지 확인하고 
>
> 두 번째로 해당 `Point`클래스가 아니면 매개변수 클래스의 *equals*를 호출한다. 
>
> (myColorPoint.equals(mySmellPoint) 에서는 mySmellPoint는 ColorPoint가 아니므로 SmellPoint의 *equals*를 호출한다.)
>
> myColorPoint.equals(mySmellPoint) => mySmellPoint.equals(myColorPoint)
>
> 그런데 이는 똑같이 구현되어 있기에 다시 두 번째에서 매개변수 클래스의 *equals*를 다시 호출한다. 이는 무한으로 반복될 것이다. 
>
> 따라서 StackOverflow가 발생한다.

<br>

그러면 어떻게 해야할까?

결론은 추상화의 이점을 포기하지 않는 한, **구체 클래스를 확장해 새로운 값을 추가하면서 *equals* 규약을 만족시킬 방법은 없다.**

<br>

그러면 `equals`안의 `instanceof`검사를 `getClass`로 바꾸면 규약도 지키고 값을 추가하면서 구체 클래스를 상속할 수 있지 않을까?

```java
@Override
public boolean equals(Object obj) {
    if (obj==null || obj.getClass() != getClass())
        return false;

    Point p = (Point) obj;
    return p.x == x && p.y == y;
}
```

같은 클래스의 객체와 비교할 때만 true를 반환한다. 하지만 이는 하위 클래스와는 비교할 수 없다. 

예를 들어 주어진 점이 반지름이 1인 단위 원 안에 있는지를 판별하는 메소드가 필요하다고 해보자. 

```java
 private static final Set<Point> unitCircle = Set.of(
     new Point(1, 0), new Point(0, 1),
     new Point(-1, 0), new Point(0, -1)
 );

public static boolean onUnitCircle(Point p) {
    return unitCircle.contains(p);
}
```

이제 값을 추가하지 않는 방식으로 `Point`를 확장하겠다.

```java
public class CounterPoint extends Point{
	
    // AtomicInteger는 동시성을 보장하므로 counter는 총 생성되는 객체의 개수
    private static final AtomicInteger counter = new AtomicInteger();

    public CounterPoint(int x, int y) {
        super(x, y);
        counter.incrementAndGet();
    }

    public static int numberCreated(){
        return counter.get();
    }
}
```

`CounterPoint`클래스는 `Point`클래스를 상속받았다. 또한 값을 따로 확장하지 않았다. 

그렇다면 `CounterPoint`의 인스턴스를 `onUnitCircle`메서드에 넘기면 어떻게 될까? 

`Point`클래스의 equals를 `getClass`를 이용해 작성했다면 `false`를 반환할 것이다. 

왜 그럴까? 

리스코프 치환 원칙에 따르면 상위 타입의 메서드는 하위 타입에서도 똑같이 동작해야 한다. `CounterPoint`는 결국 `Point`클래스라고 볼 수 있기에 이처럼 활용할 수 있어야 한다는 것이다. 

그런데 `false`를 반환한다는 것은 리스코프 치환 원칙을 위반하는 것이다.

따라서 `Point`클래스의 equals는 `getClass`가 아닌 `instanceof`로 구현해야 `onUnitCircle`메서드가 원하는대로 동작할 것이다. 

<br>

#### 어떻게 고쳐야할까?

구체 클래스의 하위 클래스에서 값을 추가할 방법은 없다. 그렇지만 다른 우회 방법이 있다. 

'상속 대신 컴포지션을 사용하라'이다.

`Point`를 상속하는 대신 `Point`를 `ColorPoint`의 private 필드로 두고 `ColorPoint`와 같은 위치의 일반 `Point`를 반환하는 `뷰(View)` 메서드를 public으로 추가하는 식이다.

코드로 보는 것이 말보다 빠르다.

```java
public class ColorPoint {
    private final Point point;
    private final Color color;

    public ColorPoint(int x, int y, Color color) {
        this.point = new Point(x, y);
        this.color = Objects.requireNonNull(color);
    }

    // ColorPoint의 Point 뷰를 반환한다.
    public Point asPoint() {
        return point;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof ColorPoint))
            return obj.equals(this);

        ColorPoint cp = (ColorPoint) obj;
        return cp.point.equals(point) && cp.color.equals(color);
    }
}
```

자바 라이브러리에도 `TimeStamp`는 `Date`를 확장한 후 필드를 추가했다.

그 결과, `TimeStamp`의 *equals*는 대칭성을 위반하여 `Collection`에 넣어 사용하면 엉뚱하게 동작할 수 있다.

이렇게 설계한 것은 실수이므로 **절대 따라해서는 안된다**.

<br>

> 📖 추상클래스의 하위 클래스라면 *equals* 규약을 지키면서도 값을 추가할 수 있다.
>
> 우린 지금까지 구체 클래스의 하위 클래스 확장인 경우를 생각했다. 
>
> 만약 상위 클래스를 직접 인스턴스로 만드는 게 불가능하다면 지금까지 이야기한 문제들은 일어나지 않는다!

<br>

### 일관성 (Consistency)

두 객체가 같다면 수정되지 않는 한 앞으로도 영원히 같아야 한다는 뜻이다. 

가변 객체라면 시점에 따라 달라질 수 있지만,

불변 객체라면 한번 다르면 끝까지 달라야 한다. 

<br>

중요한 건,

클래스가 불변이든 가변이든 **equals의 판단에 신뢰할 수 없는 자원이 끼어들게 해서는 안된다.**

신뢰할 수 없는 자원이라면 항상 똑같은 결과를 주는 것이 아닌 자원이다. 

예를 들면 네트워크 환경에 따라 URL이 달라지거나, _현재 시간(LocalDateTime.now())_와 같은 변하는 것이 있다.

<br>

이런 문제를 피하려면 *equals*는 항시 메모리에 존재하는 객체만을 사용한 결정적 계산만 수행해야 한다.

<br>

### Non-NULL

이름처럼 모든 객체가 `null`과 같지 않아야 한다는 뜻이다. 

o.equals(null)가 `true`를 반환하는 상황은 없지만, 실수로 `NullPointerException`을 던지는 코드는 흔할 것이다. 

하지만 일반 규약은 이런 경우도 허용하지 않는다. 

```java
// 명시적 null 검사 - 필요 없다.
@Override
public boolean equals(Object obj) {
    if (obj == null)
        return false;
   
}
```

왜 필요없다고 할까?

기본적으로 *equals* 메서드는 동치성을 검사하기 위함이다. 

동치성을 검사하려면 같은 객체로 형변환하고 필드를 비교한다. 

그렇다면 **매개변수가 올바른 타입인지부터 확인하면 된다. 다른 타입(null 포함)은 당연히 `false`을 반환한다.**

```java
// 묵시적 null 검사
@Override
public boolean equals(Object obj) {
    if (!(obj instanceof MyType))
        return false;
    
    MyType mt = (MyType) obj;
   
}
```

<br>

### 정리

equals 메서드의 구현 방법을 정리해보자.

1. **`==` 연산자를 사용해 입력이 자기 자신의 참조인지 확인한다.**
   - 단순한 성능 최적화용으로, 비교 작업이 복잡한 상황일 때 값어치를 할 것이다.
2. **`instanceof` 연산자로 입력이 올바른 타입인지 확인한다.**
   - equals가 정의된 클래스인 것이 보통이지만, 가끔 클래스가 구현한 인터페이스가 될 수 있다. 
   - 인터페이스를 구현한 클래스라면 equals에서 클래스가 아닌 해당 인터페이스를 사용해야 한다.
3. **입력을 올바른 타입으로 형변환한다.**
   - 2번에서 `instanceof`로 검사했기 때문에 100% 성공한다.
4. **입력 객체와 자기 자신의 대응되는 '핵심' 필드들이 모두 일치하는지 하나씩 검사한다.**

<br>

#### 주의점

- `float`, `double`을 제외한 기본 타입 필드는 **연산자 == 로 비교**
- `float`, `double` 필드는 `Float.compare`, `Double.compare`로 비교한다.
  - Float.Nan, -0.0f, 특수한 부동소수 값을 다뤄야 하기 때문이다.
  - `Float.equals`, `Double.equals`는 오토박싱을 수반할 수 있으니 성능상 좋지 않다.
- 참조 타입 필드는 **각각의 equals 메서드로 비교**
  - null도 정상 값으로 취급하는 참조 타입 필드는 `Objects.equals`을 이용하자.
- 비교하기가 아주 복잡한 필드를 가진 클래스는 **필드의 표준형을 저장한 후 표준형끼리 비교하자.**
- 필드를 비교할 때는 **다를 가능성이 더 크거나, 비교하는 비용이 더 싼 필드부터 비교하자.**
  - 비교 순서에 따라 equals의 성능을 좌우한다.
  - 파생 필드를 비교하는 쪽이 빠를 수 있다.

<br>

### 진짜 마지막

*equals*를 다 구현했다면 3가지를 확인해보자.

1. 대칭적인가?
2. 추이성이 있는가?
3. 일관적인가?

또한

- *equals*를 재정의할 땐 `hashCode`도 반드시 재정의하자.

- 너무 복잡하게 해결하려 들지 말자.

  - 필드들의 동치성만 검사해도 규약을 지킬 수 있다.
  - 별칭은 비교하지 말자.

- Object 외의 타입을 매개변수로 받지 말자.

  - ```java
    public boolean equals(MyClass o){   
    }
    ```

  - 위와 같이 정의한다면 이건 재정의가 아니라 `overloading`한 것이다.

  - 따라서 @Override Annotation을 붙이지 못한다. (컴파일 에러난다.)

  - 문제가 되더라도 원인을 찾기 쉽지 않다!

<br>

### 요약

꼭 필요한 경우가 아니면 *equals*를 재정의하지 말자. 

많은 경우에 `Object`의 *equals*가 원하는 비교를 정확히 수행해준다.

재정의할 때는 위의 5가지 규약을 확실히 지키며 비교해야 한다.

