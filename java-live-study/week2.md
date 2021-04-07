# Week 2.

이번 주는 Java Data Type, Variable, 그리고 Array에 대해 알아보자.

<br/>

### Primitive Type

Java에서는 변수를 선언할 때 타입을 지정한다. 따라서 사용할 변수의 특성에 맞는 타입을 지정해주는 것이 중요하다.

기본(Primitive) 타입은 정수, 실수, 문자, 논리 리터럴을 직접 저장하는 타입이다. 

| 값의 종류 | 기본 타입               | 메모리 사용크기 | 값의 범위                 | 기본 값  |
| --------- | ----------------------- | --------------- | ------------------------- | -------- |
| 정수      | byte                    | 1byte ( 8bit )  | -128 ~ 127 (-2^7 ~ 2^7-1) | 0        |
|           | char                    | 2byte ( 16bit ) | 0 ~ 2^16-1                | '\u0000' |
|           | short                   | 2byte ( 16bit ) | -2^15 ~ (2^15-1)          | 0        |
|           | int                     | 4byte ( 32bit ) | -2^31 ~ (2^31-1)          | 0        |
|           | unsigned int (java 8~)  | 4byte ( 32bit ) | 0 ~ 2^32-1                | 0        |
|           | long                    | 8byte ( 64bit ) | -2^63 ~ (2^63-1)          | 0L       |
|           | unsigned long (java 8~) | 8byte ( 64bit ) | 0 ~ 2^64-1                | 0L       |
| 실수      | float                   | 4byte ( 32bit ) |                           | 0.0F     |
|           | double                  | 8byte ( 64bit ) |                           | 0.0      |
| 논리      | boolean                 | 1byte ( 8bit )  | true, false               | false    |

만약 타입의 범위를 넘어가면 쓰레기 값이 저장된다. 

<br/>

간단히 값의 범위가 정해지는 원인(?)을 설명하자면,

N bit의 경우 첫 번째 bit는 부호를 나타낸다. 0이면 양수, 1이면 음수이다. 

따라서 부호를 나타내는 1bit를 제외한 (N-1)bit가 2번씩 나오므로 가능한 범위는 최소 -2^(n-1), 최대 2^(n-1)-1이 되는 것이다.

<br/>

> unsigned int, unsigned long???
>
> 원래는 없다가 java 8부터 생겼다. 
>
> 다음과 같이 사용할 수 있다.
>
> ```java
> // int의 범위를 넘어서는 22억을 사용해보자.
> int unsigned = Integer.parseUnsignedInt("2200000000");
> System.out.println(Integer.toUnsignedString(unsigned));
> ```

<br/>

#### char 타입

Java는 모든 문자를 유니코드(Unicode)로 처리한다. 따라서 유니코드를 처리하기 위해 2byte 크기인 char를 사용한다. 

> 유니코드란?
>
> 세계 각국의 문자들을 코드값으로 매핑한 국제 표준 규약. 
>
> 하나의 문자에 대해 하나의 코드값을 부여하므로 영문 'A', 한글 '가'도 코드값을 가진다. 범위는 0~65535 범위의 2byte 크기

char 변수에 값을 저장하는 법은 3가지이다.

```java
// 모두 대문자 A를 저장하는 방법이다.
// 1. 작은 따옴표로 감싼 문자
char c = 'A';
// 2. 유니코드 정수값
char c = 65;
// 3. '\u + 16진수 값'
char c = '\u0041';
```

<br/>

다만 주의할 점은 char 타입의 변수는 단 하나의 문자만 저장한다. 

문자열을 저장하기 위해서는 String 타입을 사용해야 하는데 이는 레퍼런스 타입이니 이따가 다뤄보자.

<br/>

#### long 타입

int 범위를 넘어서는 정수를 저장할 수 있다. 

주의할 점은 값을 초기화할 때, 값이 int 범위를 넘어선다면 반드시 숫자+L를 붙여야 한다. 그래야 컴파일 에러가 발생하지 않는다.

이는 컴파일러에게 8byte 수임을 알려주기 위함이다.

<br/>

#### float, double 타입

부동 소수점 방식으로 값을 저장한다. 

주의할 점은 Java는 실수 리터럴의 기본 타입을 double로 간주한다. 따라서 float타입을 저장할 때는 반드시 리터럴 뒤에 'f' or 'F' 를 붙여야 한다.

<br/>

> 📖 그 외 꿀팁
>
> ```java
> int number = 1_000_000;
> ```
>
> 와 같이 선언 가능....! (개수는 상관없이 _로 구분할 수 있다!)

<br/>

### Primitvie Type & Reference Type

Java의 데이터 타입에는 기본(primitive) 타입과 참조(reference) 타입이 있다. 이번에는 참조 타입에 대해 알아보자.

참조 타입이란 객체(Object)의 번지를 참조하는 타입으로 배열, 열거, 클래스, 인터페이스 타입을 말한다.

기본 타입은 실제 값을 변수 안에 저장하지만, 참조 타입은 객체의 메모리 주소를 값으로 저장한다. 그래서 이를 통해 객체를 참조하므로 참조 타입이다!

<br/>

예를 통해 이해하자.

```java
int stock = 10;
int price = 36000;

String title10 = "이펙티브 자바";
String author = "조슈아 블로크";
```

라는 케이스가 있다고 하자. 

이럴 때 메모리는 어떻게 될까?

![memory](https://github.com/gashe-soo/java-study/blob/master/img/lj_week2_memory.JPG?raw=true)

기본 타입은 stack 영역에서 값을 저장하고 있지만, 참조 타입 변수는 heap 영역의 객체 주소 값을 참조하고 있다.

<br/>

### 리터럴

리터럴은 소스 코드 내에서 직접 입력된 값을 뜻한다. 

종류는 정수, 실수, 문자, 논리가 있다. 

#### 

#### 정수

- 소수점이 없음 => 10진수
  - 0, 75, -100
- 0으로 시작 => 8진수
  - 02, -04
- 0x, 0X로 시작 => 16진수
  - 0x5, 0xA

#### 실수

- 소수점이 있음 => 10진수
  - 0.25, 3.14
- 대문자 E or 소문자 e => 10진수 지수와 가수
  - 5E7 (5*10^7)

#### 문자

- 작은 따옴표('')로 묶은 텍스트
  - 'A', '가'
- 역슬래쉬가 붙은 문자
  - '\t', '\n'

#### 문자열

- 큰 따옴표("")로 묶은 텍스트
  - "자바", "탭만큼 이동\t" 

#### 논리

- boolean 타입

<br/>

### 변수 선언 및 초기화

변수(Variable)는 하나의 값을 저장할 수 있는 메모리 공간이다. 

Java의 경우 데이터 타입과 변수 이름을 함께 선언한다.

```java
int num;
```

int라는 4byte의 메모리를 쓰겠다고 선언하는 것이다. 

<br/>

>  참고. 변수 명명 규칙
>
> | 작성 규칙                                                    | 예                                         |
> | ------------------------------------------------------------ | ------------------------------------------ |
> | 첫 번째 글자는 문자이거나 '$','_'이어야 하고 숫자로 시작할 수 없다. | 가능 : price, $rice<br />불가 : 1v, @speed |
> | 영어 대소문자가 구분된다.                                    | firstname과 firstName은 다르다.            |
> | 첫 문자는 영어 소문자로 시작하되, 다른 단어가 붙을 경우 첫 문자를 대문자로 한다. | firstName                                  |
> | 문자 수의 제한은 없다 / 자바 예약어는 사용할 수 없다.        |                                            |

<br/>

#### 초기화

초기화는 변수에 초기 값을 주는 것을 뜻한다.

```java
int num = 10;
int age;
age = 20;

String name = "java";
String author;
author = "joshua";
```

<br/>



### 변수의 스코프와 라이프타임

변수의 스코프란 접근이 가능하고 변수가 존재하는 영역을 뜻한다.

Java는 중괄호 {} 블록을 기준으로 변수의 스코프를 정한다.

선언된 위치에 따라 클래스 변수, 인스턴스 변수, 지역 변수로 나눌 수 있다. 

스코프가 블록인만큼 변수는 선언된 블록 내에서만 사용 가능하다.

| 변수의 종류   | 선언 위치   | 생성시기(메모리 할당 시기)  | 변수의 스코프                                                |
| ------------- | ----------- | --------------------------- | ------------------------------------------------------------ |
| 클래스 변수   | 클래스 영역 | 클래스가 메모리에 올라갈 때 | 클래스 전역(클래스 기반으로 생성된 각각의 인스턴스에 공유됨) |
| 인스턴스 변수 | 클래스 영역 | 인스턴스가 생성될 때        | 각각의 인스턴스                                              |
| 지역 변수     | 메소드 영역 | 변수 선언문이 수행되었을 때 | 메소드 안                                                    |

여기서 백기선님이 집어주신 포인트는 

<img src="https://github.com/gashe-soo/java-study/blob/master/img/lj_week2_scope.JPG?raw=true" alt="week2_scope.jpg"  />

그림과 같이 static 변수는 클래스 변수로 Class Loader가 로딩하면서 JVM의 메소드 영역에 저장된다. (위 표의 선언위치 : 메소드영역과 다르다.)

그러나 인스턴스 변수는 런타임에 실행되면서 올라간다. 따라서 static 변수가 new Instance()를 참조하는 것은 불가능하다.



<br/>

### 타입 변환

타입 변환이란 데이터 타입을 다른 데이터 타입으로 변환하는 것을 말한다. 

<br/>

#### Promotion : 자동 타입 변환

자동 타입 변환은 실행 도중에 자동적으로 타입 변환이 일어나는 것을 말한다.

자동 타입 변환은 작은 크기를 가지는 타입이 큰 크기를 가지는 타입에 저장될 때 발생한다

```
큰 크기 타입 = 작은 크기 타입
```

간단하게 생각하면 이 경우가 훨씬 쉽기에 자동적으로 일어난다. 

작은 그릇을 큰 그릇에 넣긴 쉽다. 

비트를 보자

```java
byte num = 10;
int intNum = num; // promotion
```

```
byte일 경우 : 00001010 을 가진다. 
int일 경우 : 00000000 / 00000000 / 00000000 / 00001010 이 된다.
```

값의 손실 없이 그대로 보존된다.

<br/>

> 예외는 있다. 
>
> byte -> char는 자동 변환이 안된다. 
>
> 이유는 char는 0~65534의 범위로 음수가 없다. 

<br/>

#### Casting : 강제 타입 변환

이번엔 반대다. 큰 그릇을 작은 그릇에 옮겨 담기다. 당연히 그냥은 할 수 없다. 그래서 쪼개야 한다. 

그런데 문제는 쪼개지면서 값의 손실이 일어난다는 것이다. 

예를 들어 int는 4byte인데 이를 byte에 옮겨담으려고 한다면 1byte만 저장할 수 있다. 

```java
int intNum = 103029770;
byte byteNum = (byte) intNum; 
```

으로 한다면 byteNum 은 int의 마지막 1byte인 00001010만 담게 되어 10이 된다. 

만약 실수 -> 정수 변환이라면 소수점은 버려지고 정수 부분만 저장된다.

따라서 주의점이 2개 있다.

- 값의 손실 발생 여부 검사
- 정수 => 실수 타입 변환하면서의 정밀도 손실

<br/>

#### 연산식에서의 자동 타입 변환

연산은 기본적으로 같은 타입 간에만 가능하다. 따라서 다른 타입의 연산일 경우 크기가 큰 타입으로 자동 변환된 후 연산을 수행한다. 

```java
int intNum = 10;
double doubleNum = 5.5;
double result = intNum + doubleNum; // 결과는 15.5
```

물론 doubleNum을 int로 casting하여 결과를 int로 반환할 수 있다. 

<br/>

**정수 연산의 경우 int 타입이 기본**이다. 따라서 크기가 4byte보다 작으면 int타입으로 변환되어 연산한다. 

<br/>

### 1차 및 2차 배열

배열은 같은 타입의 데이터를 연속된 공간에 나열시키고, 각 데이터에 인덱스를 부여해 놓은 자료구조이다. 

배열 선언은 다음과 같이 두 가지 형태로 작성된다.

```java
// 1. 
타입[] 변수;
// 2. 
타입 변수[];
```

배열 변수는 참조 변수에 속한다. 객체이므로 Heap 영역에 생성되고 배열 변수는 배열 객체를 참조하게 된다. 

참조할 배열 객체가 없다면 변수는 null값으로 초기화될 수 있다.

<img src="https://github.com/gashe-soo/java-study/blob/master/img/lj_week2_array.JPG?raw=true" alt="lj_week2_array.jpg"  />

위와 같이 스택과 힙 영역에서 메모리가 할당된다.

배열을 생성하는 법을 보자.

```java
// 1. 
int[] num = {1,2,3};
// 2.
int[] num;
num = new int[] {1,2,3};
// 3.
int[] num = null;
num = new int[] {1,2,3};
// 불가능한 방법
int[] num;
num = {1,2,3}; //(컴파일에러 발생)
```

배열을 생성할 때 길이를 정할 수 있다.

```java
int[] num = new int[3];
```

<br/>

#### n차원 배열

n차원 배열도 생성이 가능하다. 

어떻게 선언할까?

```java
int[][] num = {{1,2,3},{3,4,5}};
int[][] num = new int[2][3];
```

메모리는 어떻게 될까?

<img src="https://github.com/gashe-soo/java-study/blob/master/img/lj_week2_array2.JPG?raw=true" alt="lj_week2_array2.jpg"  />

힙에서 배열 안에 배열이 생성되어 다시 이를 참조한다. 

N차원이라면 계속 배열 안에 배열이 생성되어 참조한다고 생각하면 된다. 

<br/>

그러면 배열 안에 **크기가 다른 배열**도 가능할까?

**가능**하다. 

어차피 객체를 참조하는 형태이기 때문이다. 

```java
int[][] num = new int[2][];
num[0] = new int[2];
num[1] = new int[3];
```

<img src="https://github.com/gashe-soo/java-study/blob/master/img/lj_week2_array3.JPG?raw=true" alt="lj_week2_array3.jpg"  />

<br/>

### 타입 추론, var

지금까진 변수를 선언할 때 데이터 타입을 명시해야 한다고 주구장창 말해왔다. 

그런데 Java 10부터 새로운 것이 등장했다!

이는 타입 추론이다. 타입 추론이란 변수의 타입을 명시하지 않아도, 컴파일러가 변수에 저장된 리터럴을 기반으로 타입을 추론하는 것이다!

js, kotlin 등을 공부했다면 낯설지 않을 것이다. 

자바에서는 타입 추론을 var로 사용한다. 

특징을 알아보자. 

1. Var는 **초기화 값이 있는 지역변수로만** 선언이 가능하다.

   - var는 멤버변수, 또는 메소드의 파라미터, 리턴 타입으로 사용이 불가능하다. 
   - 선언할 때 초기화 값이 있어야 한다.

   <img src="https://github.com/gashe-soo/java-study/blob/master/img/lj_week2_var.JPG?raw=true" alt="lj_week2_var.jpg"  />

   보다시피 멤버변수로 선언하면 컴파일 에러가 난다. 

   <br/>

2. Var는 **런타임 오버헤드가 없다.**

   - 설명했다시피 var는 컴파일러가 체크해서 타입 추론한다. 그러면 타입 추론은 당연히 컴파일 과정에서 일어난다.
   - 그러므로 런타임에는 이미 데이터 타입이 부여된 상태이므로 당연히 런타임 오버헤드는 일어나지 않는다.

3. Var의 잘못된 사용법

   - 초기화하지 않는다. 

   - null를 할당한다.

   - 배열을 사용할 때는 데이터 타입을 명시해야 한다. 

     ```java
     var name;	// 불가
     var name = null; 	// 불가
     var arr = {1,2,3}; // 불가
     var arr = new int[]{1, 2, 3};	// 가능
     ```



<br/><br/><br/>

### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어

[lee_maru님의 블로그](https://catch-me-java.tistory.com/19)

[kksb0831님의 github](https://github.com/kksb0831/Practice_project/blob/master/Java_Study_02.md)








