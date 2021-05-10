# MultiThread

- [프로세스와 스레드](#프로세스와-스레드)
- [Thread 생성하기](#Thread-생성하기)
- [스레드 우선순위](#스레드-우선순위)
- [스레드 상태](#스레드-상태)
- [동기화](#동기화)
- [Deadlock](#Deadlock)

<br>

<br>

### 프로세스와 스레드

[여기](https://github.com/gashe-soo/OS-7week-KOCW/blob/main/week_2/Chapter-3.md)를 참고하자.

<br><br>

### Thread 생성하기

Java에서는 스레드를 만들 때 2가지 방법이 있다.

1. `Runnable` 인터페이스를 구현한 객체를 `Thread`생성자에 넣어준다. 

   `Runnable` 인터페이스는 스레드에서 실행할 코드를 가진 `run()`메소드를 가졌다.

   일반적인 방법이며, 인터페이스를 구현하면 되므로 유연성이 있다. 

   ```java
   public class HelloRunnable implements Runnable {
   
       public void run() {
           System.out.println("Hello from a thread!");
       }
   
       public static void main(String args[]) {
           (new Thread(new HelloRunnable())).start();
       }
   
   }
   ```

2. `Thread`클래스를 상속한 서브 클래스를 만든다. 

   `Thread`클래스는 `Runnable`인터페이스를 구현했다. 서브 클래스에서 `run()`메소드를 오버라이딩해서 사용하면 된다.

   간단하게 사용할 수 있지만, `Thread`의 서브 클래스이므로 제한이 있을 수 있다.

   ```java
   public class HelloThread extends Thread {
   
       public void run() {
           System.out.println("Hello from a thread!");
       }
   
       public static void main(String args[]) {
           (new HelloThread()).start();
       }
   
   }
   ```

<br>

두 방법 모두 `Thread.start`메소드를 호출하여 스레드를 실행시킨다.

> 왜 run()메소드가 아닌 start() 메소드를 실행할까?
>
> 이는 main 스레드와도 연관되는데, 자세한 건 뒤에서 설명한다. 
>
> 간단히 말하면 run()메소드는 현재 스레드가 실행할 명령을 갖고 있다. 
>
> 우리가 원한 멀티스레딩은 여러 스레드가 동시에 각자의 명령을 실행하는 것이다.
>
> 그렇게 하기 위해서는 새로운 스레드를 만들어줘야 한다. 새로 만들어주는 것이 start()메소드인 것이다.

<br>

<br>

### 스레드 우선순위

스레드의 우선순위를 말하기 전에 동시성과 병렬성에 대해 말해보자.

동시성(Concurrency)과 병렬성(Parallelism)은 언뜻 듣기에는 비슷할 것 같지만 엄연히 다르다!

동시성이란 하나의 Core가 여러 일을 번갈아 하는 것이다. Core 하나가 Thread1, Thread2, Thread1...처럼 일을 번갈아한다.

병렬성은 여러 개의 Core가 일을 하나씩 맡아서 동시에 진행하는 것이다. Core1은 Thread1, Core2는 Thread2을 맡아서 진행한다.

코어가 워낙 빠르게 일을 하니 동시성도 사용자 입장에선 병렬성처럼 보이는 것이다.

<br>

이걸 왜 언급하냐면, 아무리 빠르게 실행하더라도 동시성에서는 분명히 순서가 있다. 이것을 정해주는 것이 스케줄링이다. 

자바의 스레드 스케줄링은 2가지를 사용한다.

- 우선순위 방식 (Priority)
- Round-Robin 방식

<br>

#### 우선순위 방식

우선순위는 1~10이 부여되는데 1(낮음)~10(높음)이다. Default는 5이다. 

우선순위를 설정하기 위해서 `Thread`클래스에서 메소드를 제공한다.

```java
thread.setPriority(우선순위);
```

값이 아니라 상수를 사용할 수 있다.

```java
thread.setPriority(Thread.MAX_PRIORITY);	// 10
thread.setPriority(Thread.NORM_PRIORITY);	// 5
thread.setPriority(Thread.MIN_PRIORITY);	// 1
```

만약 병렬적으로 실행하고 코어 수 > 스레드 수라면 우선순위는 크게 영향을 끼치지 못한다.

<br>

### 스레드 상태

스레드 객체를 생성하고 `start()` 메소드를 호출한다고 바로 실행되는 것은 아니다. 

**실행 대기 상태가 되었다가 스케줄링으로 CPU를 점유하고 `run()`를 실행한다.**

실행하면 이제 `RUNNING`상태가 되는 것이다. 다시 다른 스레드가 CPU를 점유하면 실행 대기로 돌아간다. 또는 일시 정지상태로 가기도 한다.

아래는 이를 나타낸 그림이다.

![lj_multiThread_thread-state.jpg](https://github.com/gashe-soo/java-study/blob/master/img/lj_multiThread_thread-state.jpg?raw=true)

| state     | 열거 상수       | description                                                  |
| --------- | --------------- | ------------------------------------------------------------ |
| 객체 생성 | `NEW`           | 스레드 객체가 생성, 아직 start() 메소드가 호출되지 않은 상태 |
| 실행 대기 | `RUNNABLE`      | 실행 상태로 언제든지 갈 수 있는 상태                         |
| 일시 정지 | `WAITING`       | 다른 스레드가 통지(notify)할 때까지 기다리는 상태            |
|           | `TIMED_WAITING` | 주어진 시간 동안 기다리는 상태                               |
|           | `BLOCKED`       | 사용하고자 하는 객체의 락이 풀릴 때까지 기다리는 상태        |
| 종료      | `TERMINATED`    | 실행을 마친 상태                                             |

<br>

#### 상태 제어

멀티 스레드를 사용하기 위해서는 정교한 상태 제어가 필요하다. 상태 제어가 잘못되면 프로그램이 다운될 수 있다.

다음은 상태 변화를 가져오는 메소드의 종류다.

![lj_multiThread_thread-state.jpg](https://github.com/gashe-soo/java-study/blob/master/img/lj_multiThread_thread-state-control.jpg?raw=true)

| Method                                                       | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| interrupt()                                                  | 일시 정지 상태의 스레드에서 `interruptException` 예외를 발생시켜, 예외 처리 코드에서 실행 대기 상태로 가거나 종료 상태로 갈 수 있도록 한다. |
| notify()<br />notifyAll()                                    | 동기화 블록 내에서 wait() 메소드에 의해 일시 정지 상태에 있는 스레드를 실행 대기 상태로 만든다. |
| sleep(long mills)                                            | 주어진 시간동안 스레드를 일시 정지 상태로 만든다. 주어진 시간이 지나면 자동적으로 실행 대기 상태가 된다. |
| join()<br />join(long mills)<br />join(long mills, int nanos) | join() 메소드를 호출한 스레드는 일시 정지 상태가 된다. 실행 대기 상태로 가려면, join() 메소드를 멤버로 가지는 스레드가 종료되거나, 매개값으로 주어진 시간이 지나야 한다. |
| wait()<br />wait(long mills)<br />wait(long mills, int nanos) | 동기화 블록 내에서 스레드를 일시 정지 상태로 만든다. 매개값으로 주어진 시간이 지나면 자동적으로 실행 대기 상태가 된다. 시간이 주어지지 않으면 `notify()`,`notifyAll()`메소드에 의해 실행 대기 상태로 갈 수 있다. |
| yield()                                                      | 실행 중에 우선순위가 동일한 다른 스레드에게 실행을 양보하고 실행 대기 상태가 된다. |

<br>

#### sleep

실행 중인 스레드를 멈추게 하여 일시 정지 상태가 되고 다시 실행 대기 상태로 돌아간다.

```java
try{
    Thread.sleep(1000L);
}catch(InterruptException e){
    
}
```

<br>

#### yield()

다른 스레드에게 실행을 양보하고 자신은 실행 대기 상태로 돌아가도록 한다. 

아래 코드는 무의미한 반복을 하고 있다.

```java
public void run(){
    while(true){
        if(work)
            System.out.println("ThreadA work");
    }
}
```

성능을 높이기 위해서 다음과 변경할 수 있다.

```java
public void run(){
    while(true){
        if(work)
            System.out.println("ThreadA work");
        else
            Thread.yield();
    }
}
```

<br>

#### join()

스레드는 다른 스레드가 종료될 때까지 기다려야 하는 경우가 있다. 

예를 들어 계산을 나누어 하고 이를 합쳐서 결과를 내야 하는 경우, 결과값을 사용하기 위해서는 스레드를 기다려야 한다. 

```java
public class SumThread extends Thread{
    private long sum;
    
    public long getSum(){
        return sum;
    }
    
    public void setSum(long sum){
        this.sum = sum;
    }
    
    @Override
    public void run(){
        for(int i=1;i<=100;i++){
            sum += i;
        }
    }
}

public static void main(String[] args){
    SumThread sumThread = new SumThread();
    sumThread.start();
    
    try{
        sumThread.join();
    }catch(InterruptException e){}
    
    System.out.println("Sum of 1~100 : "+sumThread.getSum());
}
```

<br>

#### interrupt()

스레드는 `run()`메소드를 모두 실행되면 자동 종료된다. 

하지만 경우에 따라서는 즉시 종료할 필요가 있다. 이럴 때 `interrupt()`를 사용한다. 

이전에는 `stop()`메소드를 사용했으나 자원으들이 불안전한 상태로 남겨지기에 deprcated되었다.

<br>

interrupt()메소드는 스레드가 **일시 정지 상태에 있을 때** `InterruptedException`예외를 발생시켜 종료하게 한다.

```java
class ThreadB extends Thread{
    public void run(){
        try{
            while(true){
                Thread.sleep(1);	// 일시 정지
            }
        }catch(InterruptedException e){}
    }
}

ThreadB threadB = new ThreadB();
threadB.interrupt();
```

다음과 같은 `ThreadB`가 있을 때 `interrupt()`메소드를 호출하면 `Thread.sleep(1)`으로 일시 정지 상태가 되었을 때 `interruptedException`을 발생시켜 스레드를 종료시킨다. 

주의할 점은 일시 정지가 되어야만 종료가 가능한다. 따라서 일시 정지되는 코드가 없다면 이를 활용할 수 없다.

그래서 정적 메소드인 `interrupted()` or 인스턴스 메소드의 `isInterrupted()`를 통해 `interrupt()` 호출 여부를 확인할 수 있다.

만약 호출되었다면 스레드를 종료시키도록 코드를 작성하면 된다.

<br>

<br>

### 동기화

멀티 스레드 프로그래밍을 하다보면 여러 스레드가 공통적으로 접근하는 데이터가 생겨난다. 

그런데 스레드 A가 작업하던 데이터를 스레드 B가 바꿔버리면 스레드 A는 원하는 결과를 얻지 못하게 된다. 

은행 계좌의 돈을 인출하는 작업을 해보자. 

300원을 가진 계좌에서 각 스레드가 200원, 100원씩 인출한다는 상황을 가정하자. 

코드는 아래와 같다.

```java
    static class Account{
        private int balance;

        public Account(int balance) {
            this.balance = balance;	// 잔고
        }

        public int getBalance(){
            return balance;
        }

		// 인출
        public void withdraw(int money){
            this.balance -= money;
            try{
                Thread.sleep(2000L);	// 2초 sleep
            }catch(InterruptedException e){
                System.out.println(Thread.currentThread().getName() +" : "+this.balance);
            }
            System.out.println(Thread.currentThread().getName() +" : " + getBalance());	// 현재 스레드와 잔고 출력
        }
    }

    static class ThreadA extends Thread{

        public Account account;

        public void setAccount(Account account) {
            this.setName("threadA");
            this.account = account;
        }

        public Account getAccount() {
            return account;
        }

        @Override
        public void run() {
            account.withdraw(200);	// 200원 인출
        }
    }
    static class ThreadB extends Thread{

        public Account account;

        public void setAccount(Account account) {
            this.setName("threadB");
            this.account = account;
        }

        public Account getAccount() {
            return account;
        }

        @Override
        public void run() {
            account.withdraw(100);	// 100원 인출
        }
    }
	public static void main(String[] args) {
        Account account = new Account(300);

        ThreadA threadA = new ThreadA();
        ThreadB threadB = new ThreadB();
        threadA.setAccount(account);
        threadB.setAccount(account);

        threadA.start();	// 예상 잔고 100원
        threadB.start();	// 예상 잔고 0원
    }
```

그러나 결과는?

```
threadA : 0
threadB : 0
```

둘 다 0원이다. 이렇게 되면 크나큰 문제가 생긴다. 

따라서 critical section영역을 만들어 문제를 해결해야 한다. 

Java에서는 2가지의 방법이 존재한다. 

<br>

#### synchronized method

method에 `synchronized`라는 키워드를 붙여 사용한다.

`synchronized`키워드는 인스턴스/정적 메소드에 붙일 수 있다.

```java
public synchronized void method(){
    
}
```

다만, `synchronized`를 붙이면 해당 메소드를 실행하는 스레드는 객체의 `intrinsic lock`를 얻게 된다.

<br>

> Intrinsic lock? monitor lock?
>
> 각 객체는 고유의 락을 가지고 있다. 이를 Intrinsic lock이라 한다.
>
> 이는 객체에 대한 접근을 배타적으로 할 수 있게 해준다.

스레드는 객체의 `intrinsic lock`를 얻은 후에 메소드를 실행하고 메소드가 종료되면 반납한다.

따라서 해당 메소드를 실행하고 있는 스레드가 있다면 다른 스레드는 메소드가 종료될 때까지 기다려야 한다. 

<br>

그래서 위의 케이스를 바꿔보자.

```java
public synchronized void withdraw(int money){
	// .....            
}
```

키워드를 붙여주게 되면 결과는 다음과 같이 변한다.

```java
threadA : 100
threadB : 0
```

<br>

그러면 `synchronized` 키워드를 붙이면 다 해결할 수 있을까?

공유 객체에 대한 문제는 해결될 것이다. 

하지만 언급했다시피, synchronized method의 lock 범위는 객체 전체다. 

다음과 같이 두 개의 메소드가 있다고 가정해보자.

```java
int c1;
int c2;

public synchronized void increment1(){
    c1++;
}
public synchronized void increment2(){
    c2++;
}
```

여기서 threadA는 `increment1()`메소드를 호출하고, threadB는 `increment2()`메소드를 호출한다면? 

두 개의 스레드는 서로 다른 변수를 다루고 있고 아무런 관련이 없다. 

하지만 `synchronized` 키워드가 붙은 메소드이므로 lock이 객체 전체에 걸리게 된다. 

따라서 threadB는 threadA의 메소드 실행이 끝날 때까지 기다려야 하는 것이다.

프로그램의 성능도 당연히 저하된다.

이를 해결하는 것이 synchronized statement다.

<br>

#### synchronized statement

동기화 블록은 다음과 같이 사용된다.

```java
public void method(){
    synchronized(공유객체){
        // 단 하나의 스레드만 실행
    }
    // 여러 스레드가 실행 가능 영역
}
```

하나의 스레드만 실행이 가능하도록 해야 하는 경우, 동기화 블록 내에 선언하고

여러 스레드가 실행해도 되는 경우는 동기화 블록 외부에 선언하면 된다.

<br>

[java tutorials](https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html)에서는 다음과 같이 예제를 제공한다.

```java
public class MsLunch {
    private long c1 = 0;
    private long c2 = 0;
    private Object lock1 = new Object();
    private Object lock2 = new Object();

    public void inc1() {
        synchronized(lock1) {
            c1++;
        }
    }

    public void inc2() {
        synchronized(lock2) {
            c2++;
        }
    }
}
```

보면서 왜 두 개의 `Object` 객체를 선언했을까? 라는 의문이 들었는데,

생각해보니 메소드마다 lock을 제공하기 위함이었다. 

<br><br>

### Deadlock

Deadlock의 개념을 모른다면 [여기](https://github.com/gashe-soo/OS-7week-KOCW/blob/main/week_4/Chapter-7.md)를 참고하라.



```java
public class Deadlock {
    static class Friend {
        private final String name;
        public Friend(String name) {
            this.name = name;
        }
        public String getName() {
            return this.name;
        }
        public synchronized void bow(Friend bower) {
            System.out.format("%s: %s"
                + "  has bowed to me!%n", 
                this.name, bower.getName());
            bower.bowBack(this);
        }
        public synchronized void bowBack(Friend bower) {
            System.out.format("%s: %s"
                + " has bowed back to me!%n",
                this.name, bower.getName());
        }
    }

    public static void main(String[] args) {
        final Friend alphonse =
            new Friend("Alphonse");
        final Friend gaston =
            new Friend("Gaston");
        new Thread(new Runnable() {
            public void run() { alphonse.bow(gaston); }
        }).start();
        new Thread(new Runnable() {
            public void run() { gaston.bow(alphonse); }
        }).start();
    }
}
```

간단히 설명하자면, 

하나의 스레드가 synchornized method인 `bow(gatson)`를 호출하여 `alphonse`의 락을 얻었다. 그리고 `gaston`의 synchronized method인 `bowBack()`를 호출한다. 

그런데 `gaston`의 lock은 이미 다른 스레드가 가지고 있다. 따라서 기다려야 한다. 

하지만 이 스레드 또한 `alphonse`의 lock을 얻기를 기다리고 있으니 서로 기다리는 deadlock이 발생한 것이다.

<br><br>

### REFERENCE

'이것이 자바다' 신용권 저, 한빛미디어

[Java Tutorials- Concurrency](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)







