# Week 3. 연산자 & 제어문

<br>

### 연산자

연산(operations)이란 프로그램에서 데이터를 처리하여 결과를 산출하는 것이다. 

연산에 사용되는 표시나 기호를 연산자(operator)라 하고, 

연산되는 데이터는 피연산자(operand)라고 한다.

<br>

Java에서는 다양한 연산자를 제공하고 있다. 

| 연산자 종류 | 연산자                                             | 피연산자 수    | 산출값            | 설명                            |
| ----------- | -------------------------------------------------- | -------------- | ----------------- | ------------------------------- |
| 산술        | + - * / %                                          | 이항           | 숫자              | 사칙연산 + 나머지 계산          |
| 부호        | + -                                                | 단항           | 숫자              | 음수, 양수                      |
| 문자열      | +                                                  | 이항           | 문자열            | 문자열 연결                     |
| 대입        | =, +=, -=, *=, /=, %=, &=, ^=, \|=, <<=, >>=, >>>= | 이항           | 다양              | 우변의  값을 좌변의 변수에 대입 |
| 증감        | ++ --                                              | 단항           | 숫자              | 1만큼 증가/감소                 |
| 비교        | ==, !=, >, <, >=, <=, instanceof                   | 이항           | boolean           | 값의 비교                       |
| 논리        | ! & \| && \|\|                                     | 단항<br />이항 | boolean           | 논리적 NOT, AND, OR 연산        |
| 조건        | 조건식? A : B                                      | 삼항           | 다양              | 조건식에 따라 A, B 중 하나 선택 |
| 비트        | ~ & \| ^                                           | 단항<br />이항 | 숫자<br />boolean | 비트 NOT, AND, OR, XOR 연산     |
| Shift       | \>> << >>>                                         | 이항           | 숫자              | 비트를 좌/우측으로 밀어서 이동  |

<br>

하나씩 살펴보자.

<br>

### 연산자 우선 순위

연산자에도 우선 순위가 있다. 

| 연산자                                    | 연산 방향 | 우선순위 |
| ----------------------------------------- | --------- | -------- |
| 증감(++, --), 부호(+,-), 비트(~), 논리(!) | <=        | 1        |
| 산술(* / %)                               | =>        | 2        |
| 산술(+ -)                                 | =>        | 3        |
| Shift ( <<, >>, >>>)                      | =>        | 4        |
| 비교(< > <= >= instanceof)                | =>        | 5        |
| 비교(== !=)                               | =>        | 6        |
| 논리 &                                    | =>        | 7        |
| 논리 ^                                    | =>        | 8        |
| 논리 \|                                   | =>        | 9        |
| 논리 &&                                   | =>        | 10       |
| 논리 \|\|                                 | =>        | 11       |
| 조건 ?:                                   | =>        | 12       |
| 대입( =, +=....etc)                       | <=        | 13       |

정리하면

- 단항, 이항, 삼항 연산자순으로 우선순위
- 산술, 비교, 논리, 대입 연산자순으로 우선순위
- 단항, 대입을 제외하곤 연산 방향은 왼쪽에서 오른쪽이다.
- 괄호를 사용해서 우선순위를 정해줄 수 있다.

<br>

### 산술 연산자

산술 연산자의 특징은 피연산자들의 타입이 동일하지 않을 경우 아래 규칙에 따라 타입을 일치시킨 후 연산을 수행한다. 

- 피연산자들이 모두 정수 타입이고, int타입보다 크기가 작으면 모두 int 타입으로 변환 후 연산 수행한다. 산출 타입도 int
  - byte + byte  = int
- 피연산자들이 모두 정수 타입이고, long 타입이 있을 경우 모두 long 타입으로 변환 후 연산 수행한다. 산출 타입도 long
  - int + long = long
- 피연산자 중 실수 타입이 있을 경우, 크기가 큰 실수 타입으로 변환 후 연산 수행한다. 산출 타입은 실수 타입
  - int + double = double + double = double
  - int + float = float + float = float

따라서 의도와 다르게 연산되는 경우를 조심해야 한다.

```java
int one = 10;
int two = 4;
double result = one / two;
```

결과는 어떻게 될까? 

```java
2.0
```

2.0이 나온다. 산출 타입이 double이라 2.5를 예상했지만, 결과는 달랐다. 

왜냐하면 `one/two`의 값은 `int`로 변하면서 2가 되고 이것을 double형으로 바꿨기 때문에 `2.0`이 산출된다. 

<br>

이번에 `char` 타입을 보자.

```java
char c1 = 'A'+1;
char c2 = 'A';
char c3 = c2 +1;
```

여기서 오류가 있다. 

어디냐면 사실 c3 라인에서 컴파일에러가 난다. 

왜냐하면 c2는 `char` 타입이고 1은 정수타입이므로 c2도 `int`으로 변환하여 연산하고 

산출 타입도 int다! 따라서 char으로 할당하지 못한다. 

```java
char c3 = (char)(c2+1);
```

다음과 같이 캐스팅은 가능하다!

<br>

또한 Overflow를 조심해야 한다.

```java
int first = 2_000_000_000; // 20억
int second = 2_100_000_000; //21억

int mid = (first + second) /2;
```

혹시 코딩테스트에서 이분탐색을 많이 해봤다면 자주 봤을 유형이다.(ㅋㅋㅋ)

mid에는 어떤 값이 들어갔을까? 예상대로 2_050_000_000이 들어갔을까?

결과는 오버플로우가 나서 음수값이 들어간다. 

왜냐하면 (first + second) 부분에서 이미 41억으로 int의 범위를 넘기 때문이다. 

따라서 이는 다음과 같이 변경해야 한다. 

```java
int mid = first + (second-first)/2;

// 이는 나중에 다루자.
int mid = (first+second) >>> 1;
```

<br>

#### NaN & Infinity 연산

나누기, 나머지 연산자는 사용할 때 조심해야 한다. 

만약 나누는 수인 우측 피연산자가 0을 사용한다면 컴파일은 되지만 실행 시 ArithmeticException이 발생한다. 

```java
5 / 0 -> ArithmeticException 예외 발생
5 % 0 -> ArithmeticException 예외 발생
```

만약 우측 피연산자가 0.0f, 0.0의 실수 타입이라면 예외발생이 아닌 다른 결과 값을 준다.

```java
5 / 0.0  => Infinity (무한대)
5 % 0.0  => NaN (Not A Number)
```

그런데 문제는 NaN이나 Infinity에 산술연산을 해도 컴파일에러가 발생하지 않는다.

```java
Infinity + 2 => Infinity
NaN + 2 => NaN
```

따라서 이는 `Double.isNaN()` 과 `Double.isInfinite()`으로 판단해야 한다. 

<br>

### 비교 연산자

비교 연산자에는 `==`, `!=`,`<=`,`>=`,`<`,`>` 으로 총 6가지가 있다.

<br>

비교 연산자에서도 연산 수행하기 전에 **타입 변환을 통해 피연산자의 타입을 일치시킨다.**

```java
'A' == 65 // 'A'는 int 타입으로 변환되어 65가 되어 결과는 true
3 == 3.0 // 3은 int, 3.0은 double 타입으로 3은 double타입으로 변환되어 true
0.1 == 0.1f // 부동소수점 타입에서 0.1f은 사실상 0.1의 근사값이다. 따라서 false가 나온다.
   
(float)0.1 == 0.1f //올바른 연산
```

<br>

#### 문자열 비교

문자열 String에도 비교 연산자인 (==, !=)연산자는 사용할 수 있다. (대소 연산자는 사용 불가)

```java
String str1 = "java";
String str2 = "java";
String str3 = new String("java");
String str4 = new String(str1);

System.out.println(str1 == str2);
System.out.println(str1 == str3);
System.out.println(str1 == str4);
System.out.println(str3 == str4);
```

결과는 어떻게 나올까?

```
true
false
false
false
```

문자열은 같은 리터럴을 사용할 경우 같다고 판단하나

같은 값을 가졌다고 해도 객체가 새로 생성되면 다르다고 판단한다.

```java
System.out.println(str1.equals(str2));
System.out.println(str1.equals(str3));
System.out.println(str1.equals(str4));
System.out.println(str3.equals(str4));
```

값이 같은지 비교하는 equals를 사용해서 비교하면?

```
true
true
true
true
```

모두 `true`을 반환한다.

<br>

### 비트 연산자

비트 연산자는 데이터를 비트(bit) 단위로 연산한다. 그렇기 때문에 0과 1로 표현이 가능한 정수 타입만 비트 연산을 할 수 있다.

따라서 `float`와 `double`는 비트 연산을 할 수 없다.

<br>

#### 비트 논리 연산자

비트 논리 연산자는 `&`, `|`, `^`, `~`이 있다. 

또한, 비트 논리 연산자 피연산자를 `int`타입으로 변환한 후 연산을 수행한다. `byte`,`char`,`short`타입은 비트 논리 연산하면 결과는 `int`타입이 된다.

```java
byte num1 = 45;
byte num2 = 25;
int result = num1 & num2;
```

<br>

#### 비트 이동 연산자

비트 이동 연산자(shift)는 정수 데이터의 비트를 좌,우측으로 밀어서 이동시키는 연산을 수행한다. 

```java
a << b // 정수 a의 각 비트를 b만큼 왼쪽으로 이동
a >> b // 정수 a의 각 비트를 b만큼 오른쪽으로 이동 (빈자리는 정수 a의 최상위 부호 비트(MSB)와 같은 값으로 채워진다.)
a >>> b // a >> b와 같이 오른쪽으로 이동시키지만 빈자리가 0으로 채워진다.
```

<br>

### 논리 연산자

논리 연산자에는 논리곱(&&), 논리합(||), 배타적 논리합(^) 그리고 논리부정(!) 연산자가 있다. 

기본적으로 **피연산자는 boolean타입만 가능**하다.

<br>

#### && 와 & 의 차이

`&&`와`&`, `||`와 `|`는 무슨 차이가 있을까?

```java
int i = 0;
int j = 0;

if (i++ == 0 || j++ == 0)
    System.out.println("Guess Result");

System.out.println(i);
System.out.println(j);
```

다음과 같이 실행하면 어떤 결과가 나올까? 

```
1번
1
1

2번
1
0
```

사실 뻔하다면 이런 내용을 안 썼을 것이다. 그러므로 답은 2번이다.

왜 2번이냐?

`&&`, `||`는 `A||B`에서 A가 `true`라면 B를 확인하지 않는다. 이처럼 `&&`, `||`는 앞의 피연산자 결과에 따라 뒤 피연산자를 확인한다. 

반면 `&`, `|`은 두 피연산자의 결과를 모두 확인한 후에 결과를 연산한다.

<br>

### 3항 연산자

삼항 연산자는 세 개의 피연산자를 필요로 하는 연산자를 말한다. 

이는 ? 앞의 조건식에 따라 콜론 앞뒤에 피연산자가 선택된다. 

```java
조건식? 조건식 true일때의 피연산자 : 조건식 false일때의 피연산자
```

그래서 다음과 같이 사용할 수 있다.

```java
int score = 95;
char grade = (score>90) ? 'A' : 'B';
```

<br>

### 대입 연산자

대입 연산자는 오른쪽 피연산자의 값을 좌측 피연산자 변수에 저장한다.

대입 연산자의 경우 꽤나 많다. 간단히 표로 보자.

| 연산식 |       |          | 설명                     |
| ------ | ----- | -------- | ------------------------ |
| 변수   | =     | 피연산자 | 피연산자를 변수에 저장   |
| 변수   | +=    | 피연산자 | 변수 = 변수 + 피연산자   |
| 변수   | -=    | 피연산자 | 변수 = 변수 - 피연산자   |
| 변수   | *=    | 피연산자 | 변수 = 변수 * 피연산자   |
| 변수   | /=    | 피연산자 | 변수 = 변수 / 피연산자   |
| 변수   | %=    | 피연산자 | 변수 = 변수 % 피연산자   |
| 변수   | &=    | 피연산자 | 변수 = 변수 & 피연산자   |
| 변수   | \|=   | 피연산자 | 변수 = 변수 \| 피연산자  |
| 변수   | ^=    | 피연산자 | 변수 = 변수 ^ 피연산자   |
| 변수   | <<=   | 피연산자 | 변수 = 변수 < 피연산자   |
| 변수   | \>>=  | 피연산자 | 변수 = 변수 >> 피연산자  |
| 변수   | \>>>= | 피연산자 | 변수 = 변수 >>> 피연산자 |

<br>

#### 연산 순서

대입 연산자는 모든 연산자 중에서 가장 낮은 연산 순위를 가진다.

또한 연산의 진행 방향이 오른쪽에서 왼쪽이다.

따라서 아래 식은 다음과 같이 진행된다.

```java
int a = b = c = 5;
// 1.
c = 5;
// 2.
b = c;
// 3.
a = b;
```

<br>

 증감 연산자와 혼합되면 꽤나 혼란스러운데

```java
int i = 5;
int j = ++i;
int k = i++;

System.out.println(i);
System.out.println(j);
System.out.println(k);
```

어떻게 출력될까?

증감 연산자의 경우,

연산자가 피연산자보다 먼저 나오면 연산을 다른 연산보다 먼저 수행한다. ex. `++i`

반대로 연산자가 피연산자보다 뒤에 나오면 다른 연산을 수행한 후에 증감 연산을 한다. ex. `i++`

<br>

따라서 결과는 다음과 같다. 왜 그런지 모르겠다면 증감 연산자의 연산 순위를 다시 생각해보자.

```
7
6
6
```

<br>

### 조건문 

조건문에는 `if문`, `switch문`이 있다.

형태를 보며 익히자.

```java
// 조건식이 true일 때만 수행한다.
if(조건식){
    조건식이 true일 때 수행문
}

// if-else문
if(조건식){
    조건식이 true일 때 수행문
}else{
    조건식이 false일 때 수행문
}

//if - elseif -else문
if(조건식 1){
    조건식 1이 true일 때 수행문
}eles if(조건식 2){
    조건식 2가 true일 때 수행문
}else{
    조건식 1 및 조건식 2가 false일 때 수행
}
```

- if-elseif 문에서는 조건식 1이 false여야만 else if문이 실행된다!

<br>

#### switch 문 및 switch연산자

Java 13부터 Switch expressions이 업데이트되었다. 궁금하면 [여기](https://openjdk.java.net/jeps/354) 클릭 

원래 switch문은 

```java
switch (day){
    case 5:
        System.out.println("TGIF~!");
    case 6:
    case 7:
        System.out.println("WEEKEND");
        break;
    default:
        System.out.println("WEEKDAY");
}
```

- case 문마다 하나의 switch문에 해당하기 위해서는 `break` 가 필요했다.
- 또한 여러 case에 속해도 모두 case + 조건을 작성해야 했다.

```java
int numLetters;
switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        numLetters = 6;
        break;
    case TUESDAY:
        numLetters = 7;
        break;
    case THURSDAY:
    case SATURDAY:
        numLetters = 8;
        break;
    case WEDNESDAY:
        numLetters = 9;
        break;
    default:
        throw new IllegalStateException("Wat: " + day);
}
```

- 변수를 쓰기 위해서는 지역 변수 경우 case 안에서 끝나버리기 때문에 switch문 밖에서 선언하고 사용해야 했다.

<br>

Java 13부터는 좀 더 간단해졌다.

```java
switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY                -> System.out.println(7);
    case THURSDAY, SATURDAY     -> System.out.println(8);
    case WEDNESDAY              -> System.out.println(9);
}
// 똑같지만 화살표 대신 콜론으로도 가능하다. 다만 -> , : 같이는 사용하지 못한다.
switch (day) {
    case MONDAY, FRIDAY, SUNDAY : System.out.println(6);
    case TUESDAY                : System.out.println(7);
    case THURSDAY, SATURDAY     : System.out.println(8);
    case WEDNESDAY              : System.out.println(9);
}
```

얼마나 깔끔해졌는가... `break`도 사라졌고, 여러 케이스도 한 줄에 적을 수 있게 되었다.

게다가 변수 사용도 훨씬 쉬워졌다.

```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
};
```

너무 깔끔해졌다. 

<br>

### 반복문

반복문에서는 `for문`, `while문`, `do-while문`이 있다. 

하나씩 보자.

<br>

#### FOR 문

FOR문은 반복문을 수행한다.

```java
for(초기화식;조건식;증감식){
    실행문;
}
```

주의해야 할 점은 Loop Count Variable를 선언할 때 **부동소수점 타입을 사용하지 않아야 한다.**

```java
for(float x = 0.1f; x <= 1.0f; x += 0.1f){
    System.out.println(x);
}
```

위에서도 계속 말했지만 부동소수점 타입은 100% 확실한 값을 표현하지 않는다. 

따라서 결과가 아래처럼 나온다.

```java
0.1
0.2
0.3
0.4
0.5
0.6
0.70000005
0.8000001
0.9000001
```

10번을 의도했지만 9번밖에 나오지 않는다.

<br>

FOR문에서는 조건식에 따른 증감말고도 FOR-EACH문도 있다. 

배열이나 Collections 타입은 FOR-EACH 문으로 조회할 수 있다. 

```java
int[] arr = {1, 2, 3, 4, 5, 6, 7};

for (int num : arr) {
    System.out.println(num);
}

// 결과
1
2
3
4
5
6
7
```

<br>

#### While문

FOR문이 정해진 횟수만큼 반복한다면, WHILE문은 조건식이 true일 경우에 계속해서 반복한다.

```java
while(조건식){
	조건식이 true일동안 실행
}
```

<br>

while문에는 do-while문도 있다. 

둘의 차이는

- while문은 조건식을 검사하고 수행한다. 즉, 조건식이 거짓이면 한 번도 수행하지 않을 수 있다.
- do-while문은 실행문을 우선 실행시키고 조건식을 이후에 검사한다.

```java
do{
    실행문;
}while(조건식)
```



<br><br>

### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어





