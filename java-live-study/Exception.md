# Exception

<br>

### 예외와 에러

Java를 실행하는 과정 속에서 오류가 발생한다. Java는 이를 에러와 예외로 나누는 데 차이가 무엇일까?

<br>

컴퓨터 하드웨어의 오동작 또는 고장으로 인해 프로그램 실행 오류가 발생하는 것을 에러(error)라고 한다.

에러는 JVM 실행에 문제가 생겼으므로 결국 실행 불능이 된다. 따라서 개발자가 대처할 방법이 없다.

<br>

예외(Exception)란 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인해 발생하는 프로그램 오류이다. 

예외는 예외 처리를 통해 프로그램을 종료하지 않고 정상 실행 상태가 유지되도록 할 수 있다.

예외는 2가지 종류가 있다.

- 일반 예외
  - 실행 예외를 제외한 모든 예외
  - Checked Exception : 컴파일하는 과정에서 예외 처리 코드가 필요한 지 검사
  - 따라서 컴파일 단계에서 예외 처리가 되어있는지 확인하고 이를 확인할 수 있다. 
- 실행 예외
  - Java를 실행하면서 발생하는 예외
  - Unchecked Exception : 컴파일 과정에서 예외 처리 코드를 검사하지 않는다.

<br>

구조는 다음과 같다. 

`Throwable`

- `Error`
- `Exception`
  - `Checked Exception`
  - `Runtime Exception`

`Throwable` class를 상속받아 `Error` 클래스, `Exception`클래스가 있다.

<br><br>

### Runtime Exception

실행 예외는 컴파일 과정에서 확인하지 못하므로 개발자가 따로 처리하지 않으면 프로그램이 종료된다. 

따라서 종류를 알아두고 대처하는 것이 중요하다.

<br>

#### java.lang.NullPointerException

객체 참조가 없는 상태, 즉 `null`값을 갖는 참조 변수로 객체 접근 연산자를 사용했을 때 발생

#### java.lang.ArrayIndexOutOfBoundsException

배열에서 인덱스 범위를 초과하여 사용할 경우 발생

#### java.lang.NumberFormatException

문자열을 숫자로 변환할 때, 변환될 수 없는 문자를 변환하려고 할 때 발생

```java
int value = Integer.parseInt("a100"); 	// NumberFormatException 발생
```

#### java.lang.ClassCastException

상위-하위 클래스 or 인터페이스-구현 클래스는 타입 변환이 가능하다. 

그러나 이런 관계가 아닐 때 타입 변환하려고 한다면 불가하므로 `ClassCastException`이 발생한다.

```java
class A{}
class B extends A{}
class C extends A{}

B b = new B();
C c = new C();
change(C)

void change(A obj){
    if(obj instanceof B)
        B b = (B)obj;	// ClassCastException 가능
}
```

<br><br>

### try-catch-finally

예외가 발생했을 경우, 정상 실행을 유지할 수 있도록 처리하는 코드를 예외 처리 코드라고 한다.

try-catch-finally 블록을 이용하여 예외 처리 코드를 작성한다.

```java
try{
    // 예외 발생 가능 코드
}catch(예외클래스 e){
    // 예외 처리
}finally{
   	// 항상 실행
}
```

1. 예외 발생하지 않을 경우
   - try문 실행 이후 
   - finally 문 실행
2. 예외 발생
   - try문에서 예외 발생시
   - catch문에서 예외 처리
   - finally 문 실행

<br><br>

### 다중 catch

try 블록 내부에서 다양한 종류의 예외가 발생할 수 있다. 이럴 때는 다중 catch를 사용한다. 

```java
try{
    // 예외1 발생
    // 예외2 발생
}catch(예외1 e1){
    
}catch(예외2 e2){
    
}
```

주의할 점은 하나의 catch문만 실행된다.

예외가 발생하는 순간 실행을 멈추고 해당 catch블록으로 이동하기 때문이다.

<br>

#### catch 순서

다중 catch 블록을 작성할 때는 catch의 순서를 주의해야 한다. 

구체적 -> 포괄적 순서로 가야 한다. 왜 그래야 할까?

포괄적인 예외를 먼저 처리하게 되면 구체적인 예외 케이스에 대응하지 못하기 때문이다. 

위에서도 언급했듯이 모든 예외는 `Exception`클래스를 상속했다. 이는 모든 예외 클래스는 `Exception` 클래스로 타입 변환이 가능하다는 뜻이다.

따라서 다음과 같은 코드에선 원하는 기능을 하지 못한다.

```java
try{
    throw new ArrayIndexOutOfBoundsException();
    
    throw new NumberFormatException();
}catch(Exception e1){
    // 모두 여기서 처리하게 된다.
}catch(ArrayIndexOutOfBoundsException e2){
    // 예외 처리가 불가능하다.
}
```

따라서 순서를 바꿔야 한다.

```java
try{
    throw new ArrayIndexOutOfBoundsException();
    
    throw new NumberFormatException();
}catch(ArrayIndexOutOfBoundsException e1){

}catch(Exception e2){

}
```

<br>

#### Multi catch

Java7부터 멀티 catch가 가능하다. 

여러 예외에 대해 똑같은 처리가 가능하다면 멀티 catch 블록을 작성할 수 있다.

```java
try{
    throw new ArrayIndexOutOfBoundsException();
    throw new NumberFormatException();
}catch(ArrayIndexOutOfBoundsException e1 || NubmerFormatException e2){
    // 예외 처리
}catch(Exception e){}
```

<br><br>

### try-with-resources

try-with-resources 블록은 Java7부터 추가되었다. 

Java 라이브러리에는 `close`메소드를 호출해 직접 닫아줘야 하는 자원이 있다. ex) `InputStream`, `OutputStream`,`java.sql.Connection` 

try-with-resources 블록은 리소스를 사용 후에 `close()`메소드를 자동으로 호출해서 안전하게 리소스를 닫아준다.

<br>

이전에는 리소스를 닫기 위해 다음과 같이 사용했다.

```java
FileInputStream fis = null;
try{
    fis = new FileInputStream("file.txt");
    //...
}catch(IOException e){
    //
}finally{
    if(fis != null){
        try{
            fis.close();
        }catch(IOException e){}
    }
}
```

finally 블록에서 다시 try-catch를 사용해서 `close()`메소드를 예외 처리해야 하므로 복잡하다. 

<br>

try-with-resources를 사용하면 간략해진다.

```java
try(FileInputStream fis = new FileInputStream("file.txt")){
    //
}catch(IOException e){
    
}
```

try블록이 정상적으로 실행을 완료했거나 예외가 발생하게 되면 자동으로 `FileInputStream`의 `close()`메소드를 호출한다. 

여러 개의 리소스를 사용하면 더 편리하다

```java
try(
    FileInputStream fis = new FileInputStream("file.txt");
	FileOutputStream fos = new FileOutputStream("file2.txt")
){
    //
}catch(IOException e){
    
}
```

<br>

#### AutoClosable

try-with-resources 블록을 사용할 때 주의할 점은 사용하는 리소스가 `AutoClosable`인터페이스를 구현하고 있어야 한다.

왜냐하면 try-with-resources 블록은 `close()`메소드를 자동 호출하기 때문이다. 

<br><br>

### 예외 떠넘기기

try-catch 블록에서 예외 처리하는 것이 기본이지만, 메소드를 호출한 곳으로 예외를 떠넘길 수도 있다. 

이때 사용하는 키워드가 `throws`다.  `throws`는 메소드 선언부 끝에서 작성되어 예외를 떠넘긴다.

```java
리턴타입 메소드명(매개변수) throws 예외클래스{}
```

`throws`키워드가 붙어 있는 메소드는 반드시 try 블록 내에서 호출되어야 한다.

```java
public void method1(){
    try{
        method2();
    }catch(ClassNotFoundException e){
        //
    }
}

public void method2() throws ClassNotFoundException {
    Class clazz = Class.forName("java.lang.String2");
}
```

`method1`에서도 예외를 떠넘길 수 있다. 하지만 그렇게 되면 `method1()`를 호출하는 곳에서 try-catch로 예외를 처리해야 한다.

```java
public void method1() throws ClassNotFoundException {
    method2();
}
```

<br>

계속 예외를 떠넘기다보면 `main` 메소드에서 예외 처리를 해야 한다. 

`main`메소드도 예외를 떠넘긴다면 JVM에서 처리하고 처리 내용을 콘솔에 출력한다.

그러나 이는 권장하지 않는다. 

<br><br>

### 사용자 정의 예외

사용자가 예외를 직접 정의해서 사용할 때가 있다. 

그 전에 체크할 것이 있다. 이에 모두 해당한다면 커스텀 예외를 만들자.

- [ ] **자바 플랫폼이 제공하지 않는 예외 타입인가** 

  (Do you need an exception type that isn't represented by those in the Java platform?)

- [ ] **다른 벤더가 작성한 클래스에서 발생한 예외와 당신의 예외를 구분한다면 사용자에게 도움이 되는가?** 

  (Would it help users if they could differentiate your exceptions from those thrown by classes written by other vendors?)

- [ ] **당신의 코드가 관련 예외를 두 개 이상 발생시키는가** 

  (Does your code throw more than one related exception?)

- [ ] **누군가의 예외를 사용한다면, 사용자가 접근할 수 있는가? 비슷하게 당신의 패키지가 독립적이고 자기 포함적인가?** 

  (If you use someone else's exceptions, will users have access to those exceptions? A similar question is, should your package be independent and self-contained?)

<br>

사용자 정의 예외를 만들때는 참고하면 좋을 4가지 베스트 프랙티스가 있다. 

추천사항들은 여러분의 코드와 API를 더 이해하기 쉽게 만들어 주며 작성해야 할 문서의 양도 줄여준다. 

또한 여러 개발자가 같은 애플리케이션을 개발하거나 새로운 팀멤버가 여러분의 프로젝트에 참여하거나 그리고 외부의 소프트웨어 벤더가 여러분의 API를 사용하기로 했을 때 빠르게 성과를 내준다.

<br>

#### 1. Always Provide a Benefit

자바의 표준 예외들에 포함되지 않는 정보나 기능을 제공하는 것이 가장 중요하다.

그렇게 하지 않을 경우 JDK가 이미 제공하고 있는 방대한 수의 예외들과 비교했을 때 여러분의 커스텀 예외는 어떠한 장점도 제공하지 못하게 된다.

이렇게 여러분이 어떠한 장점 제공할 수 없는 예외를 만드는 것보다는 표준예외들 중 에서 하나를 사용하는 것이 낫다. 모든 자바 개발자들은 이 예외를 알고 있으며 여러분의 코드와 API를 다 쉽게 이해할 수 있도록 해준다.

<br>

#### 2. Follow the Naming Convention

클래스 이름이 Exception으로 끝나는 것은 일반적인 네이밍 규칙으로 자바 생태계 전체에서 사용되는 규칙이다. 

<br>

#### 3. Provide Javadoc Comments for Your Exception Class

여러분의 API의 모든 클래스, 맴버변수, 생성자들에 대해서는 문서화 하는 것이 일반적인 베스트프렉티스다. 문서화되지 않은 API들은 사용하기 매우 어렵다.

예외 클래스들은 여러분의 API에 크게 드러나지 않는 부분 일 수도 있으나 실상은 그렇지 않다. 클라이언트와 직접 관련된 메소드들 중 하나가 예외를 던지면 그 예외는 바로 예외의 일부가 된다. 그렇다는 것은 잘 만들어진 JavaDoc와 문서화가 필요하다는 뜻이다.

Javadoc은 예외가 발생할 수도 있는 상황과 예외의 일반적인 의미를 기술한다. 목적은 다른 개발자들이 여러분의 API를 이해하도록 하고 일반적인 에러상황들을 피하도록 돕는것이다.

```java
/*
* 
* The MyBusinessException wraps all checked standard Java exception and enriches them with a custom error code. 
* You can use this code to retrieve localized error messages and to link to our online documentation. 
*  
* @author TJanssen 
*/ 
public class MyBusinessException extends Exception { ... }
```

<br>

#### 4. Provide a Constructor That Sets the Cause

여러분의 코드는 커스텀 예외를 던지기 전에 표준예외을 캐치하는 케이스가 꽤 많다. 이 사실을 간과하지 말자. 보통 캐치된 예외에는 여러분이 제품에 발생한 오류를 분석하는데 필요한 중요한 정보가 포함되어 있다. 

아래의 예제를 보면 NumberFormatException은 에러에 대한 상세 정보를 제공한다. MyBusinessException의 cause처럼 cause정보를 설정하지 않으면 이 정보를 잃을 것이다.

```java
public void wrapException(String input) throws MyBusinessException {    
    try {        
        // do something    
    } catch (NumberFormatException e) {        
        throw new MyBusinessException("A message that describes the error.", e, ErrorCode.INVALID_PORT_CONFIGURATION);    
    } 
}
```

<br>

Exception과 RuntimeException은 예외의 원인을 기술하고 있는 Throwable을 받을 수 있는 생성자 메소드를 제공한다. 여러분의 예외도 이렇게 하는 것이 좋다. **발생한 Throwable를 파라미터를 통해 가져올 수 있는 생성자를 최소한 하나를 구현하고 수퍼클래스에 Throwable를 전달해줘야 한다.**

```java
public class MyBusinessException extends Exception {
    public MyBusinessException(String message, Throwable cause, ErrorCode code) {
        super(message, cause);            
        this.code = code;        
    }        
    ... 
}
```

<br><br><br>

### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어

[오라클 문서 - 예외만들기](https://docs.oracle.com/javase/tutorial/essential/exceptions/creating.html)

https://dzone.com/articles/implementing-custom-exceptions-in-java?fromrel=true

