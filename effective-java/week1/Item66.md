# Item 66. 네이티브 메서드는 신중히 사용하라



자바 네이티브 인터페이스(Java Native Interface, JNI)는 네이티브 메서드를 호출하는 기술이다. 

네이티브 메서드를 사용하는 이유는 세가지다. 

1. registry 같은 플랫폼 특화 기능 사용.
2. 네이티브 코드로 작성된 기존 라이브러리 사용
3. 성능 개선 목적

<br/>

흔히 아는 네이티브 메소드의 사용 이유다. 

그런데 왜 신중히 사용해야 할까?

<br/>

#### 플랫폼 특화 기능

자바가 성숙해가면서 플랫폼 기능을 흡수하고 있다. 

java 9부터는 process api가 추가되어 OS process에 접근할 수 있다. 

(ProcessHandle identifies and provides control of native processes.라고 쓰여있다.) [process api 보기](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ProcessHandle.html) 

물론 대체할 만한 라이브러리가 없으면 네이티브를 써야 한다!

<br/>

#### 성능 개선

JVM이 발전하면서 자바는 다른 플랫폼에 견줄만한 성능을 보인다. 

<br/>

#### 네이티브 메서드의 단점

1. 네이티브 언어가 안전하지 않으므로 네이티브 메서드를 사용하는 애플리케이션도 메모리 훼손 오류로부터 안전하지 않다.
   - Managed Language vs UnManaged Language?
2. 플랫폼을 많이 타서 이식성이 낮다.
3. 디버깅이 어렵다
4. 가비지 컬렉터가 네이티브 메모리는 자동 회수하지 못하고 추적조차 할 수 없다.
5. 자바 - 네이티브 코드를 넘나들 때마다 비용 추가된다.
6. 네이티브 메서드와 자바 코드 사이의 접착 코드가 필요하다. 

<br/>

### 요약

네이티브 메서드는 최소한만 사용하고 철저히 테스트하라. 

