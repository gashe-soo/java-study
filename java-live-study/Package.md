# Package



- [package](#Package)
- [import](#import)
- [Classpath](#Classpath)
  - [클래스패스 환경변수](#클래스패스-환경변수)
  - [-classpath 옵션](#-classpath-옵션)
- [접근지시자](#접근지시자)

<br><br>

### Package

Package(이하 패키지)는 클래스 파일의 폴더다. 다만 단순한 폴더 기능만 하는 것은 아니다.

- 클래스를 체계적으로 관리하기 위해 사용한다. 
- 클래스를 유일하게 만들어주는 식별자 역할을 한다. 

<br>

사실상 클래스는 `패키지 이름`+ `클래스 이름` 이다. 이를 `FQCN(Fully Qualified Class Name)`라고 한다. 

`pack`이란 package 아래에 `Example` class를 만들어보고 `FQCN`를 출력해보자.

```java
package pack;

public class Example {
    public static void main(String[] args) {
        Example example = new Example();
        System.out.println(example.getClass().getCanonicalName());
    }
}

// 출력 결과
pack.Example
```

위에서 `Example `class는 `pack`이란 패키지 내에 있었으므로 클래스 이름만으로도 사용이 가능했다.

`FQCN`을 이용해서도 객체를 생성할 수 있다.

```java
pack.Example example = new pack.Example();
```

<br>

#### 선언

패키지 이름은 개발자가 정해주지만, 여기도 규칙이 있다.

1. 숫자로 시작해서는 안 되고, `_`,`$`를 제외한 특수 문자를 사용해서는 안 된다.
2. java로 시작하는 패키지는 자바 표준 API에서만 사용하므로 사용해서는 안 된다.
3. 모두 소문자로 작성하는 것이 관례다.

<br>

| 이름  | 설명             |
| ----- | ---------------- |
| java  | java 기본 패키지 |
| javax | java 확장 패키지 |
| org   | 비영리단체       |
| com   | 영리단체         |

```
// EX
com.google.projectname
org.apache.projectname
```

<br>

### import

코드를 작성하다 보면 모든 패키지에 클래스를 작성할 수 없다. 따라서 패키지를 나누게 된다. 

그러면 다른 패키지의 클래스는 어떻게 사용할까?

2가지 방법이 있다.

1. 위에서 본 `FQCN`을 이용해서 객체를 선언한다. 
2. 패키지를 import하여 패키지 이름은 생략하고 클래스 이름만으로 사용한다. 

1번은 패키지 이름이 길어지거나 다양한 패키지를 사용한다면 매우 귀찮아지고 코드도 난잡해진다는 단점이 있다.

따라서 2번인 import문을 주로 사용한다. 

그림과 같이 package가 2개있고, `Example` class에서 `first` 패키지의 `First` 클래스를 사용한다고 하자. 

<img src="https://github.com/gashe-soo/java-study/blob/master/img/lj_package_tree.JPG?raw=true" alt="lj_package_tree.jpg"  />

그렇다면 아래와 같이 `import`하여 사용할 수 있다. 

<img src="https://github.com/gashe-soo/java-study/blob/master/img/lj_package_import.JPG?raw=true" alt="lj_package_import.jpg"  />

보다시피 `FQCN`을 사용하는 것보다 훨씬 깔끔해졌다.

<br>

패키지 구조를 보면 `First`라는 클래스가 각 다른 패키지에 속한다. 그러면 두 개의 클래스를 하나의 메소드에서 사용할 수 있을까?

- import를 각각 시키면 사용할 수 있지 않을까?

  ```java
  import pack.first.First;
  import pack.second.First;	// 컴파일 에러!
  ```

  위와 같이 import하면 컴파일 에러가 난다! 

  컴파일러에서 `First`라고 사용한다면 정확히 어떤 클래스를 가리키는 지 알 수 없기 때문이다.

- `FQCN`을 사용하자.

  ```java
  // FQCN 사용
  pack.first.First first = new pack.first.First();
  pack.second.First first1 = new pack.second.First();
  ```

  둘 다 import하지 않고 사용할 수 있다.

- 하나만 import하자

  ```java
  package pack;
  
  import pack.first.First;
  
  public static void main(String[] args) {
      First first = new First();
      pack.second.First first1 = new pack.second.First();
  }
  ```

  물론 `pack.second.First`를 import해도 된다.

<br>

#### 빌트-인 패키지(Built-in Package)

Built-in 패키지란 말 그대로 빌트인이 되어 있는 패키지를 뜻한다. 

java.lang 패키지는 기본적인 것이기 때문에 import로 불러오지 않아도 자바가 자동으로 java.lang의 클래스를 불러온다.

예) String, System

<br><br>

### Classpath

클래스패스는 JVM 혹은 Java 컴파일러가 사용하는 파라미터다. 이는 클래스나 패키지를 찾을 때 기준이 되는 경로다.

JVM이 소스 코드를 컴파일하면 바이트 코드로 변환된다. 

java runtime으로 이 .class 파일에 포함된 명령을 실행하려면, 먼저 이 파일을 찾을 수 있어야 한다. 

이때 .class 파일을 찾을 때 classpath에 지정된 경로를 사용한다. 

classpath는 .class 파일이 포함된 디렉토리와 파일을 콜론으로 구분한 목록이다. 

java runtime은 이 classpath에 지정된 경로를 모두 검색해서 특정 클래스에 대한 코드가 포함된 .class 파일을 찾는다. 

찾으려는 클래스 코드가 포함된 .class 파일을 찾으면 첫 번째로 찾은 파일을 사용한다.

<br>

클래스패스를 설정하는 방법은 2가지다.

<br>

#### 클래스패스 환경변수

전역 환경변수에 클래스패스를 설정할 수 있다. 

JVM이 시작될 때 JVM의 클래스 로더는 환경 변수를 사용하여 접근한다. 

해당 디렉토리가 호출되면 그 디렉토리에 있는 클래스들을 먼저 JVM에 로드한다. 

그러므로 CLASSPATH 환경 변수에는 필수 클래스들이 위치한 디렉토리를 등록하도록 한다.

<br>

#### -classpath 옵션

원하는 클래스 파일을 실행하기 위해 -classpath(-cp도 가능)으로 java runtime 중에 컴파일옵션을 준다. 

<br>

만약 `/A`, `/B`라는 path를 참조해야 한다 `;`(세미콜론)으로 구분하여 `/A;/B`처럼 사용하여 

`java -cp /A;/B /A/test.java`처럼 사용할 수 있다.

<br>

### 접근지시자

이는 이전에 [클래스-접근제어자](https://github.com/gashe-soo/java-study/blob/master/java-live-study/Class.md#접근-제한자)와 동일하다. 이를 참고하자.

<br>

<br>

### REFERENCE

[코딩하는 오징어- 자바 클래스패스란?](https://effectivesquid.tistory.com/entry/%EC%9E%90%EB%B0%94-%ED%81%B4%EB%9E%98%EC%8A%A4%ED%8C%A8%EC%8A%A4classpath%EB%9E%80)

'이것이 자바다' 신용권 저, 한빛미디어

