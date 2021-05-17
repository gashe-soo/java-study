# Annotation

<br>

### Annotation이란?

`Annotation`이란 주석과 같이 정보를 제공하는 metadata의 형태이다. `Annotation`을 달더라도 코드 작동에 직접적인 영향을 주지 않는다.

<br>

`Annotation`은 

- 컴파일러를 위한 정보 : 컴파일러가 에러를 감지하도록 한다.
- 컴파일 타임/ 배치 타임 처리 : sw툴이 코드, 파일 등을 만들수 있도록 `Annotation` 정보 처리
- 런타임 처리 : 일부 `Annotation` 들은 런타임에 처리할 수 있다.

<br>

<br>

### Annotation 형식

`Annotation`은 다음과 같이 쉽게 사용할 수 있다.

```java
@Entity
```

<br>

컴파일러에게 `@` 문자를 이용하여 `Annotation`임을 알려줄 수 있다. 

```java
@Override
void mySuperMethod() { ... }
```

`Annotation`은 구성 요소를 가질 수 있으며, 다음과 같이 사용할 수 있다.

```java
@Author(
   name = "Benjamin Franklin",
   date = "3/27/2003"
)
class MyClass { ... }
```

```java
@SuppressWarnings(value = "unchecked")
void myMethod() { ... }
```

<br>

만약에 요소가 하나 뿐이라면 생략할 수 있다.

```java
@SuppressWarnings("unchecked")
void myMethod() { ... }
```

만약 요소가 없다면 `@Override`처럼 괄호는 사용할 필요 없다. 

<br>

같은 대상에 여러 `Annotation`을 사용할 수 있다.

```java
@Author(name = "Jane Doe")
@EBook
class MyClass { ... }
```

만약 `Annotation`들이 같은 타입이라면 이를 `repeating annotation`라고 부른다.

```java
@Author(name = "Jane Doe")
@Author(name = "John Smith")
class MyClass { ... }
```

<br>

`Annotation`은 `@Override`처럼 이미 정의된 `Annotation`도 있으며, 예시처럼 Customized한 `Annotation`도 있다.

<br><br>

### Annotation을 어디에 사용할까?

`Annotation`은 클래스, 필드, 메소드 등 선언에 사용할 수 있다. 선언에 사용된다면 같은 라인에 사용된다. (Convention)

<br>

Java8부터 type 사용에 적용되었다. 

- 클래스 객체 생성

  ```java
  new @Interned MyObject();
  ```

- 타입 캐스팅

  ```java
  myString = (@NonNull String) str;
  ```

- `implements` 절

  ```java
  class UnmodifiableList<T> implements
      @Readonly List<@Readonly T> { ... }
  ```

- 예외 던지기

  ```java
  void monitorTemperature() throws
      @Critical TemperatureException { ... }
  ```

이러한 형태의 `Annotation`을 `type annotation`이라 한다.

<br><br>

### Annotation 선언

`Annotation`은 코드 내에서 주석을 대체할 수 있다.

모든 클래스에서 코드 처음에 중요 정보를 제공하는 주석을 달아야 한다고 가정하자.

```java
public class Generation3List extends Generation2List {

   // Author: John Doe
   // Date: 3/17/2002
   // Current revision: 6
   // Last modified: 4/12/2004
   // By: Jane Doe
   // Reviewers: Alice, Bill, Cindy

   // class code goes here

}
```

같은 정보를 `Annotation`으로 제공하기 위해서는 `Annotation`을 정의해야 한다. 다음과 같이 정의할 수 있다.

```java
@interface ClassPreamble {
   String author();
   String date();
   int currentRevision() default 1;
   String lastModified() default "N/A";
   String lastModifiedBy() default "N/A";
   // Note use of array
   String[] reviewers();
}
```

`Annotation` 정의는 인터페이스와 비슷하다. 다만 앞에 `@` 문자가 필요하다. `Annotation`은 인터페이스 형태다.

`Annotation`  내부에는 필요한 타입을 정의한다. 

정의하고 나면 다음과 같이 사용할 수 있다. 값도 사용할 수 있다.

```java
@ClassPreamble (
   author = "John Doe",
   date = "3/17/2002",
   currentRevision = 6,
   lastModified = "4/12/2004",
   lastModifiedBy = "Jane Doe",
   // Note array notation
   reviewers = {"Alice", "Bob", "Cindy"}
)
public class Generation3List extends Generation2List {

// class code goes here

}
```

------

> `@Documented ` annotation을 쓰면 javadoc으로 문서화를 할 수 있다.

```java
// import this to use @Documented
import java.lang.annotation.*;

@Documented
@interface ClassPreamble {

   // Annotation element definitions
   
}
```

<br><br>

### Predefined Annotation Types

일부 `Annotation`은 이미 java api에 정의되어 있다. 

<br>

`java.lang`에 정의되어 있는 `Annotation` 타입은 `@Deprecated`, `@Override`, `@SuppressWarnings`이다.

#### @Deprecated 

`@Deprecate` annotation은 해당 요소가 deprecated 되었고 더 이상 쓰지 않는다는 것을 의미한다. 

만약 `@Deprecated` annotation이 있는 클래스, 메소드, 필드 등을 사용한다면 컴파일러에서 경고를 표시한다.

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
public @interface Deprecated {
    /**
     * Returns the version in which the annotated element became deprecated.
     * The version string is in the same format and namespace as the value of
     * the {@code @since} javadoc tag. The default value is the empty
     * string.
     *
     * @return the version string
     * @since 9
     */
    String since() default "";

    /**
     * Indicates whether the annotated element is subject to removal in a
     * future version. The default value is {@code false}.
     *
     * @return whether the element is subject to removal
     * @since 9
     */
    boolean forRemoval() default false;
}
```

<br>

#### @Override

`@Override` annotation은 컴파일러에게 해당 요소가 상위 클래스의 요소를 Override했다는 것을 알려준다. 

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

Overriding할 때, annotation을 쓰지 않아도 되지만, 오류를 예방할 수 있게 도와준다. 만약 상위클래스의 메소드를 제대로 override하지 않았다면 컴파일러는 오류를 발생시킨다.

<br>

#### @SuppressWarnings

`@SuppressWarnings` annotation은 컴파일러에게 경고를 발생시키는 코드에서도 경고를 하지 않도록 한다.

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
    /**
     * The set of warnings that are to be suppressed by the compiler in the
     * annotated element.  Duplicate names are permitted.  The second and
     * successive occurrences of a name are ignored.  The presence of
     * unrecognized warning names is <i>not</i> an error: Compilers must
     * ignore any warning names they do not recognize.  They are, however,
     * free to emit a warning if an annotation contains an unrecognized
     * warning name.
     *
     * <p> The string {@code "unchecked"} is used to suppress
     * unchecked warnings. Compiler vendors should document the
     * additional warning names they support in conjunction with this
     * annotation type. They are encouraged to cooperate to ensure
     * that the same names work across multiple compilers.
     * @return the set of warnings to be suppressed
     */
    String[] value();
}
```

원래는 경고를 발생시키지만, `@SuppressWarnings` 애너테이션으로 경고를 발생시키지 않는다.

```java
// use a deprecated method and tell 
// compiler not to generate a warning
@SuppressWarnings("deprecation")
void useDeprecatedMethod() {
// deprecation warning
// - suppressed
objectOne.deprecatedMethod();
}
```

jls은 두 가지의 경고 카테고리를 제공한다. `deprecation`과 `unchecked`이다. `unchecked` 경고는 제네릭이 있기 전에 쓰여진 레거시 코드를 사용할 때 발생한다. 

여러 warnings을 suppress 하기 위해서는 다음과 같이 사용한다.

```java
@SuppressWarnings({"unchecked", "deprecation"})
```

<br>

#### @SafeVarargs

`@SafeVarargs` annotation은 가변 인자가 안전한 타입인지를 보장해준다.

<br>

#### @FunctionalInterface 

타입 선언하면서 functional interface임을 의도한 것을 알려준다. 

<br><br>

## Meta Annotation

다른 annotation에 적용될 수 있는 annotation을 meta annotation이라고 한다. 

<br>

#### @Retention

`@Retention` annotation은 해당 annotation이 얼만큼 유지되는지를 결정한다. 

- `RetentionPolicy.SOURCE` – 소스 레벨에서만 유지되고 컴파일러에서 무시된다.
- `RetentionPolicy.CLASS` – 컴파일 타임에 유지되지만, JVM에게는 무시된다.
- `RetentionPolicy.RUNTIME` – JVM에서도 유지되어 런타임에서도 사용할 수 있다.

<br>

#### @Documented 

`@Documented` annotation은 해당 annotation을 사용하면 Javadoc을 이용해 문서화 할 수 있다는 것을 알려준다.

<br>

#### @Target 

`@Target` annotation은 해당 annotation이 자바 요소에 적용될 수 있는지를 표시한다. 

종류는 다음과 같다.

- `ElementType.ANNOTATION_TYPE` 

- `ElementType.CONSTRUCTOR` 
- `ElementType.FIELD` 
- `ElementType.LOCAL_VARIABLE` 
- `ElementType.METHOD` 
- `ElementType.PACKAGE` 
- `ElementType.PARAMETER` 
- `ElementType.TYPE` can be applied to any element of a class.

<br>

#### @Inherited 

`@Inherited` annotation은 해당 annotation이 하위 클래스에도 유지되도록 한다.

```java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
public @interface Inherit {
}

@Inherit
public class Test {
}

public class ChildTest extends Test{
}
```

`ChildTest`에는 아무 annotation이 없다. 

다음과 같을 때 annotation을 가져와보자.

```java
Annotation[] annotations = ChildTest.class.getAnnotations();
for (Annotation annotation : annotations) {
    System.out.println("annotation = " + annotation);
}

// result:
annotation = @annotation.Inherit()
```

<br>

#### @Repeatable

`@Repeatable` annotation은 java 8부터 도입되었으며, 해당 annotation이 여러 번 적용할 수 있도록 한다.

```java
@Schedule(dayOfMonth="last")
@Schedule(dayOfWeek="Fri", hour="23")
public void doPeriodicCleanup() { ... }
```

어떻게 만드는 지는 [여기](https://docs.oracle.com/javase/tutorial/java/annotations/repeating.html)를 참고하라.

<br><br>

### Annotation Processor

처음에 annotation은 주석과 같이 metadata의 형태라고 말했다. 

그런데 어떤 annotation은 코드에 영향을 주기도 한다. 

예로 `@Getter`, `@Setter`를 제공하는 Lombok이 있다. 

여기서 Annotation Processor가 필요한데, Annotation을 읽고 코드를 검사, 수정, 생성하는 역할을 한다. 

<br>

Annoatation Processor는 java compile plugin으로 컴파일 타임에 annotation을 읽어 코드를 조작(?)한다. 

이를 사용하기 위해서는 `AbstractProcessor`을 상속받아 프로세서 클래스를 만들어야 한다.

만든 케이스는 잘 정리해주신 [lee ma ru님의 글](https://catch-me-java.tistory.com/49)을 참고하자.

<br><br>

### Service Loader

Service Loader는 annotation과는 다른 얘기지만, 백기선님이 언급했기에 간단하게 공부하고 넘어가자.

<br>

`Service Loader`는 서비스의 구현체를 얻기 위한 하나의 도구다. 

서비스라는 하나의 인터페이스가 있다고 가정하자. 이 서비스는 다양한 벤더들이 있어서 각각 이를 구현하고 있다. 

그래서 우리는 인터페이스를 기준으로 구현체를 사용하고 싶다. 

그럴때 Service Loader를 사용할 수 있다. 

예를 들어서

```java
package com.example;
public interface CodecFactory {
    Encoder getEncoder(String encodingName);
    Decoder getDecoder(String encodingName);
}
```

인터페이스가 있고 이를 구현한 여러 구현체가 있다고 가정하자.

ServiceLoader를 이용해 가져와보자.

```java
 ServiceLoader<CodecFactory> loader = ServiceLoader.load(CodecFactory.class);
for (CodecFactory factory : loader) {
    Encoder enc = factory.getEncoder("PNG");
    if (enc != null)
        ... use enc to encode a PNG file
        break;
}
```

다음과 같이 사용할 수 있다. 

더 자세한 경우는 [여기](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html)를 참고하라.

<br>

그런데 왜 annotation을 이야기하다 ServiceLoader로 빠졌을까? 

이유는 service loader 설정하는 부분이 annotation processor랑 비슷하기 때문이다. 

둘 다  META-INF/services 설정을 통해 등록하기 때문이다. 

일단 들어서 알아두는게 모르는 것보다 낫다!

<br>

<br>

### REFERENCE

[java-tutorials- Annotation](https://docs.oracle.com/javase/tutorial/java/annotations/index.html)

[java-docs - AbstractProcessor](https://docs.oracle.com/javase/8/docs/api/javax/annotation/processing/AbstractProcessor.html)

[lee_maru - Lombok @Getter, @Setter 직접 만들어 보자](https://catch-me-java.tistory.com/49) 

