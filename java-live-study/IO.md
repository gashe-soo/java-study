# I/O

<br>

### I/O and Stream

프로그램에서 데이터를 읽고 출력하는 작업이 있을 수 있다. 

데이터를 읽고 출력하는 작업을 Input / Output이라하여 I/O라 한다.

Java에서는 데이터를 스트림(Stream)을 통해 입출력한다. 

> Stream
>
> 단일 방향으로 연속적으로 흘러가는 것.
>
> 스트림은 다양한 데이터를 지원한다. (simple bytes, primitive data types, localized characters, and object)

<br>

#### Stream의 종류

스트림은 프로그램의 입력/출력 여부에 따라 종류가 결정된다. 

데이터를 입력받으면 InputStream, 출력하면 OutputStream이 된다. 

또한, Stream은 단방향이므로 하나의 스트림으로 입출력 모두 할 수 없다.

Stream은 두 가지 종류가 있다. 

- Byte 스트림 : 그림, 멀티미디어, 문자 등 모든 종류 지원
  - 최상위 클래스 : InputStream, OutputStream
- Character 스트림 : 문자만 가능
  - 최상위 클래스 : Reader, Writer

스트림의 종류에 맞게 최상위클래스를 상속받아 사용한다. 

예를 들어 그림 파일을 바이트 단위로 읽어들일 때에는 `FileInputStream`을 사용,

텍스트 파일을 문자 단위로 저장할 때에는 `FileWriter`를 사용한다.

<br>

<br>

### InputStream

`InputStream`은 바이트 기반 입력 스트림의 최상위 클래스로 추상 클래스이다. 

`InputStream`을 상속하는 클래스로는

- `FileInputStream`
- `BufferedInputStream`
- `DataInputStream`

주요 메서드로는

| return type | method                           | description                                                  |
| ----------- | -------------------------------- | ------------------------------------------------------------ |
| int         | read()                           | 입력 스트림으로부터 1byte를 읽고 읽은 byte를 리턴한다.       |
| int         | read(byte[] b)                   | 입력 스트림으로부터 읽은 바이트들을 매개값으로 주어진 바이트 배열 b에 저장하고 실제로 읽은 바이트 수를 리턴한다. |
| int         | read(byte[] b, int off, int len) | 입력 스트림으로부터 len개의 바이트만큼 읽고 매개값으로 주어진 배열 b[off]부터 len개까지 저장한다. 그리고 실제로 읽은 바이트 수인 len개를 리턴한다. len개를 모두 읽지 못하면 실제 읽은 바이트 수를 리턴한다. |
| void        | close()                          | 사용한 시스템 자원을 반납하고 입력 스트림을 닫는다.          |

#### read()

1byte를 읽고 4byte int 타입으로 리턴한다. 따라서 4byte 중 끝의 1byte에만 데이터가 들어있다. 

더 이상 읽을 수 없다면 -1을 리턴한다.

```java
InputStream is = new FileInputStream('test.jpg');
int readByte;
while((readByte=is.read()) !=-1){...}
```

<br>

#### read(byte[] b)

배열 크기만큼 읽는다. 만약 바이트 배열의 길이가 3이고 5개의 바이트가 들어온다면 3, 2 로 두 번 읽어들인다.

역시 더 이상 읽을 수 없다면 -1를 리턴한다.

```java
InputStream is = new FileInputStream('test.jpg');
int readByteNo;
byte[] readBytes = new byte[100];
while((readByteNo=is.read(readBytes)) !=-1){...}
```

<br>

#### read(byte[] b, int off, int len)

len개를 읽고 저장하고 리턴한다. 



> 참고
>
> read()만 추상 메서드이다. 또한, 다른 오버로딩된 메서드도 내부에선 read()를 사용한다. 
>
> 즉, read()는 필수로 구현해야 하는 메서드이다.

<br>

### OutputStream

`OutputStream`은 byte 출력 스트림의 최상위 클래스이자 추상클래스이다. 

`OutputStream`을 상속받는 클래스는 아래와 같다.

- `FileOutputStream`
- `PrintStream`
- `BufferedOutputStream`
- `DataOutputStream`

<br>

주요 메서드로는 

| return type | method                            | description                                                  |
| ----------- | --------------------------------- | ------------------------------------------------------------ |
| void        | write(int b)                      | 출력 스트림으로 1byte를 보낸다.                              |
| void        | write(byte[] b)                   | 출력 스트림으로 바이트 배열 b의 모든 바이트를 보낸다.        |
| void        | write(byte[] b, int off, int len) | 출력 스트림으로 주어진 바이트 배열 b[off]부터 len개까지의 바이트를 보낸다. |
| void        | flush()                           | 버퍼에 잔류하는 모든 바이트를 출력한다.                      |
| void        | close()                           | 사용한 시스템 자원을 반납하고 출력 스트림을 닫는다.          |

<br>

### Reader & Writer

`Reader` / `Writer`는 문자 기반 스트림의 최상위 클래스로 추상 클래스이다.

#### Reader

상속받는 클래스는 다음과 같다.

- `FileReader`
- `BufferedReader`
- `InputStreamReader`

| return type | method                              | description                                                  |
| ----------- | ----------------------------------- | ------------------------------------------------------------ |
| int         | read()                              | 입력 스트림으로부터 한 개의 문자를 읽고 리턴한다.            |
| int         | read(char[] cbuf)                   | 입력 스트림으로부터 읽은 문자들을 매개값으로 주어진 문자 배열 cbuf에 저장하고 실제로 읽은 문자 수를 리턴한다. |
| int         | read(char[] cbuf, int off, int len) | 입력 스트림으로부터 len개의 문자를 읽고 매개값으로 주어진 문자 배열 cbuf[off]부터 len개까지 저장한다. 그리고 실제로 읽은 문자 수인 len개를 리턴한다. |
| void        | close()                             | 사용한 시스템 자원을 반납하고 입력 스트림을 닫는다.          |

문자는 기본적으로 2byte다. 나머지는 `InputStream`과 동일하게 동작한다.

<br>

#### Writer

상속받는 클래스는 다음과 같다.

- `FileWriter`
- `BufferedWriter`
- `PrintWriter`
- `OutputStreamWriter`

| return type | method                               | description                                                  |
| ----------- | ------------------------------------ | ------------------------------------------------------------ |
| void        | write(int c)                         | 출력 스트림으로 주어진 한 문자를 보낸다.                     |
| void        | write(char[] cbuf)                   | 출력 스트림으로 문자 배열 cbuf의 모든 문자를 보낸다.         |
| void        | write(char[] cbuf, int off, int len) | 출력 스트림으로 주어진 문자배열 cbuf[off]부터 len개까지의 문자를 보낸다. |
| void        | write(String str)                    | 출력 스트림으로 주어진 문자열을 전부 보낸다.                 |
| void        | write(String str, int off, int len)  | 출력 스트림으로 주어진 문자열 off 순번부터 len개까지의 문자를 보낸다. |
| void        | flush()                              | 버퍼에 잔류하는 모든 문자열을 출력한다.                      |
| void        | close()                              | 사용한 시스템 자원을 반납하고 출력 스트림을 닫는다.          |

<br><br>

### 표준 스트림

#### System.in

java는 프로그램이 콘솔로부터 데이터를 입력받을 수 있도록 `System` 클래스의 `in` 정적 필드를 제공하고 있다.

```java
public final class System {

    public static final InputStream in = null;

    public static final PrintStream out = null;

    public static final PrintStream err = null;
}
```

`in`필드는 `InputStream` 타입이므로 다음과 같이 사용할 수 있다.

```java
InputStream is = System.in;
```

그러므로 콘솔로부터 데이터를 입력받을 때는 `InputStream`의 `read()`메서드를 사용하면 된다. 

만약 단어를 입력받기 위해서는 

```java
byte[] byteData = new byte[15];
int readByteNo = System.in.read(byteData);

String data = new String(byteData, 0, readByteNo-2); // 마지막 두 개의 byte는 enter 키가 차지하므로 이를 제외.
```

그런데 우리는 직접 `System`클래스를 사용하기보다는 `Scanner`클래스를 이용해 다음과 같이 입력받았다.

```java
Scanner scanner = new Scanner(System.in);

String str = scanner.nextLine();
```

그래서 `Scanner`클래스를 확인해보니 아래와 같은 생성자가 있었다. 

```java
public Scanner(InputStream source) {
    this(new InputStreamReader(source), WHITESPACE_PATTERN);
}

private Scanner(Readable source, Pattern pattern) {
    assert source != null : "source should not be null";
    assert pattern != null : "pattern should not be null";
    this.source = source;
    delimPattern = pattern;
    buf = CharBuffer.allocate(BUFFER_SIZE);
    buf.limit(0);
    matcher = delimPattern.matcher(buf);
    matcher.useTransparentBounds(true);
    matcher.useAnchoringBounds(false);
    useLocale(Locale.getDefault(Locale.Category.FORMAT));
}
```

public 생성자로 `InputStream` 클래스를 받으면 private 생성자로 클래스를 생성한다.

그래서 내부에서는 `InputStreamReader`를 이용해 생성한다.

참고로 `InputStreamReader`클래스는 `Readable`인터페이스를 구현했다.

<br>

#### System.out

`System`의 정적 필드인 `out`은 `PrintStream`으로 되어 있다. 

그래서 java에서 쓰던 출력인 `System.out.println()`메서드는 사실 `PrintStream`의 `println()`메서드를 사용한 것이다. 

`PrintStream`도 `OutputStream`을 상속받은 것이므로 `write()`, `flush()`을 이용하여 콘솔에 출력할 수 있다.

```java
OutputStream os = System.out;

for(byte b = 48;b<58;b++){
    os.write(b);
}
os.flush();
```

```java
PrintStream ps = System.out;
ps.println();
// 아래와 동일
System.out.println();
```

<br>

#### System.err

`System.err`도 `out`과 동일하게 `PrintStream`을 사용한다. 

그런데 무슨 차이인가?

```java
System.out.println("it's out");
System.err.println("it's err");
```

직접 출력해보니 달랐다. 무엇이 달랐냐면,

![lj_io_system-err.jpg](https://github.com/gashe-soo/java-study/blob/master/img/lj_io_system-err.jpg?raw=true)

색이 달랐다. `System.err`는 에러 메세지로 출력된다. 다만, 이건 내가 IDE에서 실행했기 다른 것이다. 콘솔에서는 똑같은 색으로 출력된다.

그러면 무엇이 다를까? 

[stackoverflow](https://stackoverflow.com/questions/3163399/difference-between-system-out-println-and-system-err-println)에 따르면 error 메세지를 내가 원하는 곳에 저장할 수 있다는 것이다. 

예를 들어 코드가 정상 실행일 경우 출력하고, 에러 발생할 경우 에러 메세지를 파일로 저장할 수 있다는 것이다.

<br><br>

### File 입출력

java.io에서 제공하는 `File`클래스는 입출력을 지원하지 않는다. 따라서 스트림을 사용해야 한다. 

#### FileInputStream

위에서도 언급했다시피 바이트 기반 입력 스트림이다. 

```java
FileInputStream fis = new FileInputStream("image.jpg");

File file = new File("image.jpg");
FileInputStream = new FileInputStream(file);
```

혹시 [예외 처리](https://github.com/gashe-soo/java-study/blob/master/java-live-study/Exception.md)에서 공부했던 try-with-resources 블록이 기억나는가? 이럴 때 사용하면 된다.

```java
try(FileInputStream fis = new FileInputStream("image.jpg")){
    int data;
    while((data = fis.read()) !=-1){
        System.out.write(data);
    }
}catch(Exception e){
    e.printStackTrace();
}
```

<br>

#### FileOutputStream

데이터를 파일에 저장하는 바이트 기반 출력 스트림이다.

```java
FileOutputStream fos = new FileOutputStream("image.jpg");

byte[] data = ...;
fos.write(data);
fos.flush();
fos.close();
```

<br>

#### FileReader

텍스트 파일을 프로그램으로 읽어들일 때 사용하는 문자 기반 스트림이다. 

```java
try(FileReader fr = new FileReader("test.txt")){
    int readCharNo;
    char[] cbuf = new char[100];
    while((readCharNo = fr.read()) !=-1){
		String data = new String(cbuf, 0, readCharNo);
        System.out.println(data);
    }
}catch(Exception e){
    e.printStackTrace();
}
```

<br>

#### FileWriter

문자 기반 출력 스트림이다.

```java
FileWriter fw = new FileWriter("test.txt");
String data = "test text";
fw.write(data);
fw.flush();
fw.close();
```

<br><br>

### 보조 스트림

보조 스트림이란 다른 스트림과 연결되어 여러 가지 편리한 기능을 제공해주는 스트림을 말한다. 

보조 스트림은 자체적으로 입출력을 수행할 수 없기 때문에 입출력 소스와 연결되는 클래스에 연결해서 사용해야 한다.

다음과 같이 사용한다. 아래 그림은 `이것이 자바다` 책 p1028에 나온 그림을 직접 그려서 사용했다.

![lj_io_sub-stream.jpg](https://github.com/gashe-soo/java-study/blob/master/img/lj_io_sub-stream.jpg?raw=true)

보조스트림은 하나가 아니라 다른 보조스트림에도 연결하여 스트림 체인을 만들 수 있다.

<br>

따라서 다음과 같이 선언한다.

```java
보조스트림 변수 = new 보조스트림(연결스트림);

InputStream is = System.in;
InputStreamReader reader = new InputStreamReader(is);
```

어? 아까 보지 않았나 싶을 것이다. 맞다. 위에서 `Scanner`클래스를 다룰 때 생성자에서 `new InputStreamReader(inputstream)`라는 소스코드가 있었다. 이는 보조스트림을 이용한 것이다.

<br>

간단하게 살펴보자.

#### InputStreamReader

바이트 입력 스트림에 연결되어 문자 입력 스트림인 Reader로 변환시킨다.

```java
Reader reader = new InputStreamReader(System.in);
```

<br>

#### OutputStreamWriter

바이트 출력 스트림에 연결되어 문자 출력 스트림인 Writer로 변환시킨다.

```java
Writer writer = new OutputStream(new FileOutputStream("img.jpg"));
```

<br><br>

### 성능 향상 보조스트림

프로그램의 성능은 가장 늦은 장치의 성능을 따라간다. 

따라서 CPU와 메모리가 아무리 빨라도 하드디스크의 IO가 늦어지면 프로그램 실행도 늦어진다. 

<br>

우리가 하드 디스크에 바이트를 하나씩 쓴다고 해보자. 이는 System Call이 일어나게 되므로 하나의 바이트를 쓸 때마다 비싼 비용을 치르고 있다.

그러면 어떻게 하면 성능을 높일 수 있을까? 한 번에 모아서 보내면 좀 더 빠르지 않을까? 

메모리 버퍼를 이용하면 성능을 향상 시킬 수 있다.

보조 스트림 중에서는 이처럼 메모리 버퍼를 제공하는 스트림이 있다. 

- 바이트 기반 스트림 : `BufferedInputStream`, `BufferedOutputStream`

- 문자 기반 스트림 : `BufferedReader`, `BufferedWriter`

<br>

#### BufferedInputStream & BufferedReader

이는 각각 바이트, 문자 입력 스트림에 연결되어 버퍼를 제공해주는 보조 스트림이다. 

입력 소스으로부터 자신의 내부 버퍼 크기만큼 데이터를 미리 읽고 버퍼에 저장해둔다. 프로그램은 버퍼로부터 읽음으로써 읽기 성능이 향상된다.

```java
BufferedInputStream bis = new BufferedInputStream(바이트 입력 스트림);
BufferedReader bis = new BufferedReader(문자 입력 스트림);
```

<br>

#### BufferedOutputStream & BufferedWriter

이는 각각 바이트, 문자 출력 스트림에 연결되어 버퍼를 제공해주는 보조 스트림이다. 

프로그램으로부터 자신의 내부 버퍼 크기만큼 데이터를 미리 쓰고 버퍼에 저장해둔다. 버퍼가 꽉 차면, 모든 데이터를 한 번에 보낸다.

```java
BufferedInputStream bis = new BufferedInputStream(바이트 입력 스트림);
BufferedReader bis = new BufferedReader(문자 입력 스트림);
```

<br>

Buffered과 같은 보조 스트림은 `DataInputStream`, `PrintStream`도 있다. 

<br><br>

### 객체 입출력 보조 스트림

Java는 객체를 파일 또는 네트워크로 출력할 수 있다. 객체는 문자가 아니기 때문에 바이트 기반 스트림으로 출력해야 한다. 

객체를 출력하기 위해서는 객체의 데이터 값을 일렬로 늘어선 연속적인 바이트로 변경해야 하는데, 이를 객체 직렬화(serialization)라고 한다.

반대로 입력받으면서 바이트를 객체로 복원하는 것을 역직렬화(deserialization)라고 한다.

<br>

#### ObjectInputStream, ObjectOutputStream

Java는 객체를 입출력할 수 있는 보조스트림인 `ObjectInputStream`, `ObjectOutputStream`을 제공한다. 

```java
ObjectInputStream ois = new ObjectInputStream(바이트입력스트림);
ObjectOutputStream oos = new ObjectOutputStream(바이트출력스트림);
```

객체를 직렬화하기 위해서는 `writeObject()` 메서드를 사용한다.

```java
oos.writeObject(객체);
```

읽어들일 때는 `Object`타입으로 읽으므로 타입으로 변환해야 한다.

```java
String str = (String)ois.readObject();
```

Example

```java
try(FileOutputStream fos = new FileOutputStream("image.dat");
    ObjectOutputStream oos = new ObjectOutputStream(fos)){
    oos.writeObject(new String("java"));

    oos.flush();            
}catch(Exception e){

}
try(FileInputStream fis = new FileInputStream("image.dat");
    ObjectInputStream ois = new ObjectInputStream(fis)){
    String str = (String) ois.readObject();
}catch(Exception e){

}
```

<br>

#### Serializable

Java는 `Serializable` 인터페이스를 구현한 클래스만 직렬화할 수 있도록 제한하고 있다.

이는 직렬화할 때 `private`필드를 포함한 모든 필드를 바이트로 변환해도 좋다는 표시 역할을 한다.

<br>

객체를 직렬화하면 바이트로 변환되는 것은 필드뿐이고, 생성자+ 메서드는 직렬화에 포함되지 않는다. 따라서 역직렬화할 때는 필드의 값만 복원된다.

다만, `static` or `transient` 필드의 경우 직렬화되지 않는다.

<br>

또한, 역직렬화하기 위해서는 직렬화 대상이 된 객체의 클래스가 클래스 패스에 존재해야 하며 import 되어야 한다.

- 중요한 점은 직렬화와 역직렬화를 진행하는 시스템이 서로 다를 수 있다는 것을 반드시 고려해야 한다.

<br>

#### serialVersionUID

직렬화한 객체를 역직렬화할 때는 같은 클래스를 사용해야 한다. 

만약 클래스의 이름이 같더라도 내용이 변경되면, 역직렬화는 실패한다. 

왜일까?

이유는 직렬화할 때와 역직렬할 때 사용된 클래스의 serialVersionUID가 다르기 때문이다. 

serialVersionUID는 같은 클래스임을 알려주는 식별자 역할을 하는데, 

직렬화 가능한 클래스를 컴파일하면 자동적으로 serialVersionUID가 정적 필드로 추가된다. 

serialVersionUID는 클래스의 정보를 기반으로 해쉬값으로 만들어 사용한다.

따라서 클래스 내용이 변경되어 재컴파일하면 당연히 해쉬값도 달라지므로 serialVersionUID도 달라진다.

<br>

그러므로 이 문제를 해결하기 위해서는 serialVersionUID를 명시적으로 선언해주면 된다. 

serialVersionUID는 클래스마다 다른 값으로 갖게 하는 것이 좋다. 

이미 이 문제에 대해 고민한 글이 있다. [자바 직렬화, 그것이 알고 싶다. 실무편](https://woowabros.github.io/experience/2017/10/17/java-serialize2.html)을 참고하자.

<br>

<br>

### NIO

java 1.4부터 새로운 입출력(New Input/Output)이라는 뜻에 java.nio패키지가 포함되었다. 

<br>

#### IO 와 NIO

데이터 입출력이라는 목적은 동일하지만, 방식에 있어서 차이가 난다.

| 구분            | IO          | NIO                  |
| --------------- | ----------- | -------------------- |
| 입출력 방식     | 스트림 방식 | 채널 방식            |
| 버퍼 방식       | non-buffer  | buffer               |
| 비동기 방식     | 지원 안 함  | 지원                 |
| 블로킹/넌블로킹 | 블로킹      | 블로킹/넌블로킹 지원 |

차이점에 대해 하나씩 알아보자.

<br>

#### Stream vs Channel

스트림은 단방향으로 입/출력 스트림으로 구분된다. 

채널은 스트림과 달리 양방향으로 입출력이 가능하다. 그렇기 때문에 입출력에 대한 채널이 각각 필요없고 하나의 채널로 모두 가능하다.

<br>

#### Non-Buffer vs Buffer

IO에서는 출력 스트림이 1byte를 쓰면 입력 스트림이 1byte를 읽는다. 기본적으로 느리다. 

따라서 위에서 언급했다시피 성능을 높이기 위해 버퍼를 제공해주는 보조 스트림을 사용한다. 

IO는 스트림에서 읽은 데이터를 즉시 처리한다. 그렇기 때문에 스트림으로부터 입력된 데이터를 별도로 저장하지 않으면, 입력된 데이터의 위치를 이동해 가면서 자유롭게 이용할 수 없다.

반면, NIO은 기본적으로 버퍼를 사용해 입출력한다. 따라서 IO보다는 입출력 성능이 좋다. 또한, 버퍼 내에서 데이터의 위치를 이동하면서 필요한 부분만 읽고 쓸 수 있다.

<br>

#### Blocking vs Non-Blocking

IO의 경우 입력 스트림의 read()를 호출하면 블로킹된다. 따라서 IO스레드는 그동안 다른 일을 할 수 없다. 인터럽트도 할 수 없다. 블로킹을 빠져나오기 위해서는 스트림을 닫는 것이다. 

반면 NIO은 블로킹이 되더라도 스레드에서 인터럽트함으로써 빠져나올 수 있다. 또한 넌블로킹을 지원한다. 

NIO 넌블로킹의 핵심 객체는 멀티플렉서(Multiplexor)인 selector이다. selector는 복수 개의 채널 중에서 준비 완료된 채널을 선택하는 방법을 제공한다.

<br>

#### 그러면 무엇을 선택해야 할까?

NIO가 장점이 많아보여서 모든 경우에 NIO을 선택할 필요는 없다. 

NIO는 연결 클라이언트 수가 많고, 하나의 입출력 처리 작업이 오래 걸리지 않는 경우에 사용하는 것이 좋다. 

대용량 데이터를 처리할 경우에는 IO가 더 유리하다. NIO의 경우 버퍼의 할당 크기 문제가 있으며 버퍼의 처리 방식도 IO보다 복잡하다. 

<br><br><br>

### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어

[자바 직렬화, 그것이 알고 싶다. 실무편](https://woowabros.github.io/experience/2017/10/17/java-serialize2.html)

[stackoverflow - difference between system.out.println and system.err.println](https://stackoverflow.com/questions/3163399/difference-between-system-out-println-and-system-err-println)