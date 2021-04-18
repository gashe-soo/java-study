# Item 11. equals를 재정의하려거든 hashCode도 재정의하라

<br>

Item 10에서 `equals`를 어떻게 재정의하는 지를 알아봤다. 

이번에는 `hashCode`이다.

아이템의 제목처럼 equals를 재정의하려거든 `hashCode`도 재정의해야 한다. 

이유는? `HashMap`, `HashSet`에서 hashcode를 이용해 객체의 동일함을 검사하기 때문이다!

<br>

### hashCode 규약

지난번처럼 `Object`명세의 규약을 보자.

```
Returns a hash code value for the object. This method is supported for the benefit of hash tables such as those provided by java.util.HashMap.

The general contract of hashCode is:
- Whenever it is invoked on the same object more than once during an execution of a Java application, the hashCode method must consistently return the same integer, provided no information used in equals comparisons on the object is modified. This integer need not remain consistent from one execution of an application to another execution of the same application.
- If two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce the same integer result.
- It is not required that if two objects are unequal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce distinct integer results. However, the programmer should be aware that producing distinct integer results for unequal objects may improve the performance of hash tables.

As much as is reasonably practical, the hashCode method defined by class Object does return distinct integers for distinct objects. (The hashCode may or may not be implemented as some function of an object's memory address at some point in time.)

Returns: a hash code value for this object.
```

번역을 보자면

```
- equals 비교에 사용되는 정보가 변경되지 않았다면, 애플리케이션이 실행되는 동안 그 객체의 hashCode 메서드는 몇 번을 호출해도 일관되게 항상 같은 값을 반환해야 한다. 단, 애플리케이션을 다시 실행한다면 이 값이 달라져도 상관없다.
- equals(Object)가 두 객체를 같다고 판단했다면, 두 객체의 hashCode는 똑같은 값을 반환해야 한다.
- equals(Object)가 두 객체를 다르다고 판단했더라도, 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다. 단, 다른 객체에 대해서는 다른 값을 반환해야 해시테이블의 성능이 좋아진다.
```

`hashCode` 재정의를 잘못했을 때 크게 문제가 되는 조항은 2번째다. 즉, **논리적으로 같은 객체는 같은 해시코드를 반환**해야 한다.

<br>

다음과 같은 클래스로 예를 들어보자.

```java
public final class PhoneNumber{
    private final short areaCode, prefix, linNum;
    
    public PhoneNumber(int areaCode, int prefix, int lineNum){
        this.areaCode = rangeCheck(areaCode, 999, "지역코드");
        this.prefix = rangeCheck(prefix, 999, "프리픽스");
        this.linNum = rangeCheck(linNum, 999, "가입자 번호");
    }
    
    private static short rangeCheck(int val, int max, String arg){
        if (val < 0 || val > max)
            throw new IllegalArgumentException(arg + ": " + val);
        return (short) val;
    }
}
```

```java
Map<PhoneNumber, String> m = new HashMap<>();
m.put(new PhoneNumber(707, 867, 5309), "제니");
```

`hashCode`가 재정의되어 있지 않다면?

다음과 같은 경우에 어떤 결과가 나올까?

```java
m.get(new PhoneNumber(707, 867, 5309))
```

예상대로라면 `"제니"`가 나와야 하지만, 실제로는 null를 반환한다. 

왜냐하면 `hashCode`가 재정의되지 않아 서로 다른 hashcode를 반환하기 때문이다.

참고로 `HashMap`은 해시코드가 다른 객체끼리는 동치성 비교를 시도조차 하지 않는다.

<br>

### 재정의는 어떻게 해야할까?

그렇다면 어떻게 재정의해야 할까?

아래와 같이 재정의하면 괜찮을까?

```java
@Override
public int hashCode(){
    return 42;
}
```

가능은 하지만 절대 사용해서는 안된다. 

왜냐하면 어떤 객체이든 다 동일한 해시코드를 반환하기 때문이다. 

그러면 해시테이블의 평균 수행 시간이 `O(n)`으로 느려져서 사용하는 의미가 없게 된다. 

<br>

`hashCode`의 3번째 규약처럼 

좋은 해시함수는 **다른 인스턴스에 다른 해시코드를 반환**한다. 

이는 인스턴스들을 32비트 정수 범위에 균일하게 분배해야 한다.

다음은 좋은 `hashCode`를 작성하는 요령이다.

1. int 변수 result를 선언한 후 값 c로 초기화한다. 

   이 때 c는 해당 객체의 첫번째 핵심 필드를 단계 2.1 방식으로 계산한 해시코드다.

2. 해당 객체의 나머지 핵심 필드를 f 각각에 대해 다음 작업을 수행한다.

   1. 해당 필드의 해시코드 c를 계산한다.

      - 기본 타입 필드라면, *Type*.hashCode(f)를 수행한다. (*Type*이란 해당 기본 타입의 박싱 클래스다. ex. int -> Integer.hashCode())

      - 참조 타입 필드면서 이 클래스의 `equals`메서드가 이 필드의 `equals`를 재귀적으로 호출해 비교한다면, 이 필드의 `hashCode`를 재귀적으로 호출한다. 

        계산이 더 복잡해질 것 같으면, 이 필드의 표준형을 만들어 표준형의 hashCode를 호출한다. 필드의 값이 null이면 단순 상수(보통 0)를 사용한다. 

      - 필드가 배열이라면 핵심 원소 각각을 별도 필드처럼 다룬다. 이상의 규칙을 재귀적으로 적용해 각 핵심 원소의 해시코드를 계산한 다음, 단계 2.2 방식으로 갱신한다. 배열에 핵심 원소가 하나도 없다면 단순히 상수를 사용한다. 모든 원소가 핵심 원소라면 `Arrays.hashCode`를 사용한다.

   2. 단계 2.1에서 계산한 해시코드 c로 result를 갱신한다. 

      ```java
      result = 31 * result + c;
      ```

3. result를 반환한다.

<br>

- 다른 필드로부터 계산해낼 수 있는 파생필드는 해시코드 계산에서 제외해도 된다. 
- equals 비교에 사용되지 않은 필드는 '반드시' 제외해야 한다.

<br>

단계 2.2의 곱셈 31 * result는 필드를 곱하는 순서에 따라 result값이 달라진다. 

그런데 왜 31일까?

- 31은 홀수이면서 소수(prime)이기 때문이다.
- 소수를 곱하는 이유는 명확하지 않지만 전통적으로 그리 해왔다.
- 31은 (32-1)로 31 * i 는 ( i << 5 ) - i로 shift와 빼기만으로도 최적화할 수 있다. 즉, 속도가 빨라진다는 것이다. 

<br>

`PhoneNumber`의 `hashCode`를 재정의해보자.

```java
@Override
public int hashCode(){
    int result = Short.hashCode(areaCode);
    result = 31 * result + Short.hashCode(prefix);
    result = 31 * result + Short.hashCode(lineNum);
    return result;
}
```

<br>

### hashCode 메서드

자바 플랫폼 라이브러리의 클래스들이 제공하는 hashCode메서드와 비교해도 충분히 좋다.

<br>

다음은 참고할만한 다른 방법들이다.

<br>

#### 기타 라이브러리

만약에 좀 더 해시 충돌이 더욱 적은 방법을 써야 한다면 [구아바 라이브러리](https://guava.dev/releases/21.0/api/docs/com/google/common/hash/Hashing.html)를 참고하라.

<br>

#### Objects.hash

`Objects`클래스는 임의의 개수만큼 객체를 받아 해시코드를 계산해주는 정적 메서드인 `hash`를 제공한다. 다만 속도는 더 느리다. 

```java
@Override
public int hashCode(){
    return Objects.hash(lineNum, prefix, areaCode);
}
```

```java
// Objects.hash
public static int hash(Object... values) {
    return Arrays.hashCode(values);
}

// Arrays.hashCode
public static int hashCode(Object a[]) {
    if (a == null)
        return 0;
    
    int result = 1;
    
    for (Object element : a)
        result = 31 * result + (element == null ? 0 : element.hashCode());
    
    return result;
}
```

`Arrays.hashCode`를 보면 위에서 설명했던 방식과 동일한데 왜 더 느릴까?

매개변수를 보면, Object 배열이다. 따라서 기본 타입이 섞여있다면 이를 박싱/언박싱해줘야 하기 때문이다.

<br>

#### 캐싱

또한, 클래스가 불변이고 해시코드를 계산하는 비용이 크다면 새로 계산하기보다는 캐싱하는 방식을 고려하자.

인스턴스가 만들어질 때 해시코드를 계산해둔다. 

```java
private int hashCode; // 자동으로 0으로 초기화

@Override
public int hashCode(){
    int result = hashCode;
    if (result == 0){
        result = Short.hashCode(areaCode);
        result = 31 * result + Short.hashCode(prefix);
        result = 31 * result + Short.hashCode(lineNum);
      	hashCode = result
    }
    return result;
}
```

다만 `hashCode`필드의 초기값은 흔히 생성되는 객체의 해시코드와는 달라야 한다!

<br>



### 주의할 점

#### 핵심 필드를 생략하지 말자.

성능을 높이기 위해 해시코드를 계산할 때 핵심 필드를 생략해서는 안된다. 

속도는 빨라질 수 있어도, 해시 품질이 나빠져 해시테이블의 성능을 심각하게 떨어뜨릴 수도 있다.

**특정 영역에 몰린 인스턴스들의 해시코드를 넓은 범위로 고르게 퍼트려주는 효과가 있는 필드를 생략한다면 해시테이블의 속도는 선형으로 느려진다!**

실제로 자바 2 전의 `String`은 최대 16개의 문자만으로 해시코드를 계산했다. 문자열이 길면 균일하게 나눠 16문자만 뽑아내 사용했다. 

그러나 이런 해시 함수는 URL처럼 계층적인 이름을 대량으로 사용한다면 문제가 발생한다.

```
/effectivejava/item/hashcode
/effectivejava/item/equals 
```

<br>

#### hashCode가 반환하는 값의 생성규칙을 API 사용자에게 공표하지 말자.

hashCode가 반환하는 값의 생성규칙을 API 사용자에게 자세히 공표하지 말자. 

왜 그래야할까? 

그래야 클라이언트가 이 값을 의지하지 않게 된다. 또한 추후에 계산 방식도 바꿀 수 있다.

만약 규칙을 공표해서 클라이언트가 이 값을 사용해 무언가를 한다면? 

계산 방식을 바꾼다면 클라이언트 코드에선 문제가 발생할 것이다. 

**공표하지 않아야 향후 릴리스에서 해시 기능을 개선할 여지가 생긴다!**

<br>

### 결론

`equals`를 재정의할 때는 `hashCode`도 반드시 재정의하자!







