# Item 68. 일반적으로 통용되는 명명 규칙을 따르라



### JLS (Java Language Specification)

자바는 명명 규칙 정립이 잘 되어있다. 따라서 [여기](https://docs.oracle.com/javase/specs/jls/se11/html/index.html)를 확인해보자.

자바의 명명 규칙은 크게 철자와 문법으로 나뉜다. 하나씩 살펴보자.

<br/>

### 철자

철자 규칙은 패키지, 클래스, 인터페이스, 메서드, 필드, 타입 변수의 이름을 규정한다.

모든 언어가 그렇지만 이 규칙은 무조건 따라야 한다. 

어긴다면 가독성이 떨어지고 유지보수가 하기 어려우며, **오류까지 발생**할 수 있기 때문이다.

하나씩 뜯어보자.

<br/>

#### 패키지/모듈

- 각 요소를 .(점)으로 구분하여 짓는다. 

- 요소들은 소문자 or 숫자로 명명한다. 
- 조직의 도메인 이름을 역순으로 사용한다. ex) com.google
- 패키지는 패키지를 설명하는 하나 이상의 요소로 이뤄진다. 
- 각 요소는 일반적으로 8자 이하의 짧은 단어를 사용한다. (utilities -> util / 혹은 약어로 사용)
- 하부 패키지를 만들어 계층을 구성할 수 있다.

<br/>

> Unnamed Package ( Default Package)
>
> Unnamed Package란 이름 그대로 명명되지 않은 패키지이다. 특별히 명시하지 않으면 Unnamed Package에 속한다.
>
> 그런데 이걸 왜 언급할까. 
>
> 명명규칙과는 상관없는 얘기지만, Unnamed Package에 속한 클래스는 다른 패키지에서 이를 사용할 수 없다. 
>
> 간단한 예제를 보자.
>
> <img src="https://github.com/gashe-soo/java-study/blob/master/img/ej_week1_sourceTree.JPG?raw=true" alt="week1_sourceTree.jpg"  />
>
> 소스 구조는 다음과 같다. 
>
> PackageTest에 Main method가 있어 다른 패키지에 있는 클래스를  사용할 것이다. 
>
> 다른 Class의 코드를 보자. 사용할 클래스는 print()란 메서드를 통해 자신이 어떤 클래스인지 알려준다.
>
> ```java
> public class UnnamedPackage {
> 
>     public void print(){
>         System.out.println("it's unnamed package class");
>     }
> }
> ```
>
> ```java
> package test;
> 
> public class Test {
> 
>     public void print(){
>         System.out.println("it's test class");
>     }
> 
> }
> ```
>
> 이를 활용한 클래스는 다음과 같다.
>
> <img src="https://github.com/gashe-soo/java-study/blob/master/img/ej_week1_source.JPG?raw=true" alt="week1_source.jpg"  />
>
> 보다시피 패키지에 속한 Test 클래스는 사용할 수 있지만, UnnamedPackage에 속한 UnnamedPackage 클래스는 사용할 수 없다.
>
> 그러면 같은 패키지에 속한 SamePackage에선 사용할 수 있을까? 이번엔 SamePackage에 메인 메서드로 만들어 사용해보자.
>
> <img src="https://github.com/gashe-soo/java-study/blob/master/img/ej_week1_source2.JPG?raw=true" alt="week1_source2.jpg"  />
>
> 사용 가능하다.

<br/>

#### 클래스/인터페이스

- 하나 이상의 단어로 이뤄지며 대문자로 시작한다.
- 여러 단어의 첫 글자만 딴 약어나, max,min과 같이 통용되는 줄임말을 제외하곤 **줄여쓰지 않는다.**
- 약자의 경우 첫 글자만 대문자로 하는 쪽이 더 많다. 여러 약자를 합치는 경우 가독성이 높다. ex) HTTPURL => HttpUrl

<br/>

#### 메서드/필드

- 단어 시작은 소문자로, 그 외는 클래스와 동일
- 다만, 첫 단어가 약자라면 모두 소문자! ex) http, url

<br/>

- **상수필드**
  - 모두 대문자로 사용한다
  - 단어 사이는 밑줄(_, underscore)로 구분
- 지역 변수도 다른 멤버와 동일하다
- 타입 매개변수는 한 문자로 표시한다. 
  - T : 임의의 타입엔 T (Type)
  - E : 컬렉션 원소의 타입에는 E (Element)
  - K, V : Map의 키와 값엔 K (Key) & V (Value)
  - X : 예외에는 X (eXception)
  - R : 메서드의 반환 타입에는 R (Return)

<br/>

### 문법

문법 규칙은 철자 규칙보다 더 유연하고 논란이 많다. 

<br/>

- 클래스
  - 객체를 생성할 수 있는 클래스 => 단수 명사 or 명사구 (Thread, PriorityQueue)
  - 객체를 생성할 수 없는 클래스 => 복수형 명사 (Collectors, Collections)
- 인터페이스
  - 클래스와 동일
  - able, ible로 끝나는 형용사로 짓는다. (Runnable, Accessible)
- Annotation
  - 다양하게 활용되어 지배적인 규칙은 없다
- 메서드
  - 동사나, 동사구를 사용한다. (append, drawImage)
  - boolean값을 반환하는 메서드라면 is / has 로 시작하고 명사, 명사구로 짓는다.
  - 반환값이 boolean값이 아니거나 인스턴스의 속성이라면 명사, 명사구, get로 시작하는 동사구로 짓는다. (size, getTime)
  - get으로 시작하는 형태는 주로 JavaBeans에서 나왔다. 이런 도구와 어우러지는 코드를 작성한다면 규칙을 따라도 상관없다.
  - **특별한 메서드 이름**
    - 객체의 타입을 바꿔서 다른 타입의 객체를 반환하는 메서드 => toType  ( ex. toArray, toString )
    - 객체의 내용을 다른 뷰로 보여주는 메서드 => asType ( ex. asList )
    - 객체의 값을 기본 타입 값으로 반환 => typeValue (ex. intValue )
    - 정적 팩터리의 이름
      - from, of, valueOf, instance, getInstance ....
- 필드
  - 필드는 다른 규칙에 비해 덜 명확하고 덜 중요하다.
  - boolean타입은 주로 boolean 접근자 메서드 앞에서 단어를 뺀 형태이다. ( ex. isComposite => composite)
  - 그 외는 명사 or 명사구 주로 사용



<br/>