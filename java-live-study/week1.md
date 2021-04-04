# JVM



## Java, JDK, JRE, JVM?

<br/>

#### What is Java?

Java를 공부하려면 제일 먼저 만나는 것은 JDK이다. 

우리는 JDK를 버전에 따라 설치하고 사용한다. 

그런데 설치하러 가보면 JDK,JRE도 있고 이건 Java와 무슨 상관인가? 

먼저, Java의 사상(?)부터 알아보자. 

Java의 사상(?)은 Write Once, Run Anywhere이다. 

기존의 C,C++의 경우는 컴파일하면 OS에 맞는 네이티브 코드를 생성하므로 이를 다른 OS에서 사용하지 못한다. 

Java는 이런 문제를 JVM(Java Virtual Machine)를 통해 해결한다. 

JVM는 자바 가상 머신으로 자바 바이트 코드(.class 파일)를 OS에 특화된 코드로 변환(인터프리터와 JIT 컴파일러)하여 실행한다. 바이트 코드를 실행하는 표준(JVM 자체는 표준)이자 구현체(특정 밴더가 구현한 JVM)다. 

즉, JVM만 있으면 OS에 상관없이 바이트 코드를 재사용할 수 있는 것이다. 그래서 Write Once, Run Anywhere이란 사상을 충족시킨다.

![jdl, jre, jvm](https://www.intexsoft.com/images/intexsoft/blog/JVM/jvm1.png)

[출처 : https://www.intexsoft.com/blog/post/jvm.html ]

<br/>

#### 그래서?

그래서 JRE와 JDK는 무엇이 다른 것인가? 

JRE(Java Runtime Environment)는 이름처럼 자바 실행환경으로 JVM + Java Class Library으로 이루어졌다. 그러므로 JRE만 있어도 자바 애플리케이션을 '실행'할 수 있다. 하지만 개발에 필요한 툴, 예를 들어 컴파일러인 javac는 포함하지 않는다. 

JDK(Java Development Kit)는 이름처럼 정직하게 개발 툴을 갖고 있다. JDK는 JRE+ 개발툴으로 구성되어 있다. 

그런데 JDK를 찾아보면 Oracle JDK도 있고, Amazon도 있고 OpenJDK도 있다. 이건 무엇이 다른가?

위에서 말했듯이 JVM는 표준인데 이를 구현하는 기업들이 여러 개가 있는 것이다. 밴더사마다 다르게 구현해서 JDK를 내놓는 것이다. 예전에 많이 들리던 자바 유료화 얘기는 오라클에서 제공하는 JDK가 유료화된다는 것이었는데, Java 자체가 유료가 된다고 와전된 것이다. 

> 오라클은 자바 11부터는 JDK만 제공하며 JRE를 따로 제공하지 않는다.

<br/>

#### JVM 언어

JVM 기반으로 동작하는 프로그래밍 언어가 몇 있다.

- 클로저, 그루비, JRuby, Jython, Kotlin, Scala etc

<br/>

<br/>

## JVM 구조



![jvm](https://github.com/gashe-soo/TheJava-Series/blob/main/img/code-manipulation-jvm.jpg?raw=true)

출처 : 백기선님 강의 자료

<br/>

### JVM의 동작 순서

Java 소스코드를 컴파일하면 이제 실행해야 한다. 알다시피 실행하기 위해서는 JVM을 통해야 한다. 

JVM의 동작 순서를 알아보자.

<br/>

### Class Loader

Java Code를 컴파일하면 .class라는 바이트 코드가 생겨난다. 이런 코드를 읽고 메모리에 저장하는 역할이 Class Loader이다. 

클래스 로더는 3가지를 한다. 로딩, 링크, 초기화 순으로 진행된다.

- 로딩 : 클래스를 읽어온다. .class 파일을 읽고 그 내용에 따라 적절한 바이너리 데이터를 만들고 “메소드” 영역에 저장.

  - 이때 메소드 영역에 저장하는 데이터
    - FQCN(Fully Qualified Class Name) (ex.) java.lang.String s = new java.lang.String("example"); 
    - 클래스, 인터페이스, ENUM
    - 메소드와 변수
  - 로딩이 끝나면 해당 클래스 타입의 Class 객체를 생성하여 “힙" 영역에 저장.

- 링크 : reference를 연결한다. 링크는 Verify, Prepare, Reolve(optional) 세 단계로 나눠져 있다.

  - Verify : .class 파일 형식이 유효한지 체크한다.
  - Preparation: 클래스 변수(static 변수)와 기본값에 필요한 메모리
  - Resolve: 심볼릭 메모리 레퍼런스를 메소드 영역에 있는 실제 레퍼런스로 교체한다.

- 초기화 : static 값 초기화 및 변수에 할당

  <br/>


클래스 로더는 계층 구조로 이뤄져 있으면 기본적으로 세가지 클래스 로더가 제공된다.

- 부트 스트랩 클래스 로더
  - JAVA_HOME\lib에 있는 코어 자바 API를 제공한다. 최상위 우선순위를 가진 클래스 로더

- 플랫폼 클래스로더
  - JAVA_HOME\lib\ext 폴더 또는 java.ext.dirs 시스템 변수에 해당하는 위치에 있는 클래스를 읽는다.

- 애플리케이션 클래스로더
  - 애플리케이션 클래스패스(애플리케이션 실행할 때 주는 -classpath 옵션 또는 java.class.path 환경 변수의 값에 해당하는 위치)에서 클래스를 읽는다

만약 3개의 클래스로더가 클래스를 찾지 못했다면 유명한 ClassNotFoundException이 발생한다!

<br/>

### Memory

**메소드 영역**

클래스 로더가 로딩한 클래스의 정보(클래스 이름, 부모 클래스 이름, 메소드, 변수)를 저장한다. 

**힙 영역**

객체를 저장한다. 

OS에서 배웠겠지만 힙 영역은 모든 쓰레드가 공유한다. 메소드 영역도 동일하다.

<br/>

**스택 영역**

쓰레드마다 Runtime Stack을 만들고, 그 안에 메소드 호출을 스택 프레임이라 부르는 블럭으로 쌓는다. 쓰레드 종료하면 런타임 스택도 사라진다.

<br/>

**PC(Program Counter) 레지스터**

쓰레드마다 쓰레드 내 현재 실행할 스택 프레임을 가리키는 포인터가 생성된다.

<br/>

**네이티브 메소드 스택**

네이티브 메소드를 사용할 때 쓰이는 스택이다. 

> 🔎네이티브 메소드(Native Method)이란?
>
> JAVA가 아닌 다른 언어로 작성된 메소드를 뜻한다. 보통 C/C++로 구현한다. 
>
> 그렇다면 왜 사용할까?
>
> - 다른 언어로 쓰여진 시스템 콜, 라이브러리를 구현하기 위해
> - 다른 언어로만 접근가능한 시스템, 하드웨어 자원을 사용하기 위해
> - C/C++로 쓰여진 이미 존재하는 레거시 코드를 통합하기 위해 
>
> 그렇다고 한다. 알아만 두자.

<br/>

### 실행 엔진

자 이제 메모리에 다 올려놨으니 실행해보자.

**인터프리터**

JVM은 인터프리터를 통해 바이트 코드를 한 줄씩 실행한다. 그런데 알다시피 인터프리터는 한 줄씩 실행하므로 한 번에 컴파일하는 컴파일러보다 실행 속도가 느리다. 

그래서 JVM은 이를 JIT 컴파일러를 통해 속도를 향상한다. JIT 컴파일러(Just In Time Compiler)는 인터프리터 효율을 높이기 위해, **인터프리터가 반복되는 코드**를 발견하면 **JIT 컴파일러로 반복되는 코드를 모두 네이티브 코드로 바꿔둔다**. 그 다음부터 인터프리터는 네이티브 코드로 컴파일된 코드를 바로 사용한다. 따라서 빠르게 실행할 수 있다.

<br/>

**Garbage Collector**

이번엔 GC(Garbage Collector)를 보자.

GC는 더이상 참조되지 않는 객체를 모아서 정리한다. ??? 무슨 말일까

```java
String example = new String("123");
```

위와 같은 코드를 만들면 example은 "123"이라는 값을 가진 객체를 참조하고 있다. 

그런데,

```java
example = new String("456");
```

이렇게 코드를 바꾸면 example은 "456" 값의 객체를 참조하고 이전에 참조하고 있던 **"123"값의 객체는 더 이상 참조되지 않는다.**

만약 이렇게 참조되지 않는 객체가 계속 생긴다면 힙 영역의 메모리가 over될 것이다. 그러므로 이를 관리해주는 것이 GC이다. 

GC의 실행 방법은 공부할 게 또 많으니 다음에 알아보자.

<br/>

#### ETC

**JNI(Java Native Interface)**
자바 애플리케이션에서 C, C++, 어셈블리로 작성된 함수를 사용할 수 있는 방법을 제공한다. (위에서 이미 설명했다!)

**네이티브 메소드 라이브러리**
C, C++로 작성된 라이브러리이다.

<br/>



이렇게 JVM의 구조와 실행 방법을 알아봤다. 






