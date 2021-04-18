# Item 58. 전통적인 for문보다는 for-each문을 사용하라



<br>

### 전통적인 for문

전통적인 for문으로 컬렉션을 순회하는 코드다.

```java
for(Iterator<Element> i = c.iterator();i.hasNext();){
    Element e = i.next();
}
```

전통적인 for문으로 배열을 순회하는 코드다.

```java
for(int i = 0; i < a.length; i++){
    // do something with a[i]
}
```

<br>

반복자와 인덱스 변수는 코드를 지저분하게 할 뿐 우리에게 진짜 필요한 건 원소다. 

쓰이는 요소 종류가 늘어나면 오류가 생길 가능성이 높아진다. 

또한, 컬렉션이냐 배열이냐에 따라 코드 형태가 달라지므로 주의해야 한다.

<br>

### for-each문을 사용하라

for-each문의 정식 이름은 '향상된 for문(enhanced for statement)'이다. 

- 반복자와 인덱스 변수를 사용하지 않아 코드가 깔끔해지고 오류가 날 일 없다.
- 하나의 관용구로 컬렉션/배열을 모두 처리할 수 있다.

```java
for (Element e : elements){
    //do something....
}
```

여기서 콜론(:)은 '안의(in)'으로 읽으면 된다. 

<br>

for-each문은 코드의 가독성이나 통일성에 좋다. 그런데 과연 성능도 좋을까?

다음과 같이 컬렉션에 1000개의 원소를 넣고 전통적인 for문 / for-each문으로 출력하는 코드를 작성했다.

```java
List<Integer> list = new ArrayList<>();

for (int i = 0; i < 1000; i++) {
    list.add(i);
}

// 전통적인 for문
for (Iterator<Integer> iter = list.iterator();iter.hasNext();) {
    System.out.print(iter.next());
}
// for-each문
for (int num : list) {
    System.out.print(num);
}
```

이제 Java compiler가 이를 컴파일하여 byte code로 바꾸고 jvm이 실행하도록 할 것이다. 

컴파일하여 발생한 byte code는 다음과 같다. (참고로 해석하진 않는다. 뒤에서 마저 설명하겠다. )

```
public static main([Ljava/lang/String;)V
   L0
    LINENUMBER 8 L0
    NEW java/util/ArrayList
    DUP
    INVOKESPECIAL java/util/ArrayList.<init> ()V
    ASTORE 1
   L1
    LINENUMBER 10 L1
    ICONST_0
    ISTORE 2
   L2
   FRAME APPEND [java/util/List I]
    ILOAD 2
    SIPUSH 1000
    IF_ICMPGE L3
   L4
    LINENUMBER 11 L4
    ALOAD 1
    ILOAD 2
    INVOKESTATIC java/lang/Integer.valueOf (I)Ljava/lang/Integer;
    INVOKEINTERFACE java/util/List.add (Ljava/lang/Object;)Z (itf)
    POP
   L5
    LINENUMBER 10 L5
    IINC 2 1
    GOTO L2
   L3
    LINENUMBER 14 L3
   FRAME CHOP 1
    ALOAD 1
    INVOKEINTERFACE java/util/List.iterator ()Ljava/util/Iterator; (itf)
    ASTORE 2
   L6
   FRAME APPEND [java/util/Iterator]
    ALOAD 2
    INVOKEINTERFACE java/util/Iterator.hasNext ()Z (itf)
    IFEQ L7
   L8
    LINENUMBER 15 L8
    GETSTATIC java/lang/System.out : Ljava/io/PrintStream;
    ALOAD 2
    INVOKEINTERFACE java/util/Iterator.next ()Ljava/lang/Object; (itf)
    INVOKEVIRTUAL java/io/PrintStream.print (Ljava/lang/Object;)V
    GOTO L6
   L7
    LINENUMBER 18 L7
   FRAME CHOP 1
    ALOAD 1
    INVOKEINTERFACE java/util/List.iterator ()Ljava/util/Iterator; (itf)
    ASTORE 2
   L9
   FRAME APPEND [java/util/Iterator]
    ALOAD 2
    INVOKEINTERFACE java/util/Iterator.hasNext ()Z (itf)
    IFEQ L10
    ALOAD 2
    INVOKEINTERFACE java/util/Iterator.next ()Ljava/lang/Object; (itf)
    CHECKCAST java/lang/Integer
    INVOKEVIRTUAL java/lang/Integer.intValue ()I
    ISTORE 3
   L11
    LINENUMBER 19 L11
    GETSTATIC java/lang/System.out : Ljava/io/PrintStream;
    ILOAD 3
    INVOKEVIRTUAL java/io/PrintStream.print (I)V
   L12
    LINENUMBER 20 L12
    GOTO L9
   L10
    LINENUMBER 36 L10
   FRAME CHOP 1
    RETURN
   L13
    LOCALVARIABLE i I L2 L3 2
    LOCALVARIABLE iter Ljava/util/Iterator; L6 L7 2
    // signature Ljava/util/Iterator<Ljava/lang/Integer;>;
    // declaration: iter extends java.util.Iterator<java.lang.Integer>
    LOCALVARIABLE num I L11 L12 3
    LOCALVARIABLE args [Ljava/lang/String; L0 L13 0
    LOCALVARIABLE list Ljava/util/List; L1 L13 1
    // signature Ljava/util/List<Ljava/lang/Integer;>;
    // declaration: list extends java.util.List<java.lang.Integer>
    MAXSTACK = 2
    MAXLOCALS = 4
}
```

무슨 소린지는 내가 jvm이 아니라 모르겠으니 Intellij가 Decompile해준 코드를 보자.



<img src="https://github.com/gashe-soo/java-study/blob/master/img/ej_week3_item58_decompile.JPG?raw=true" alt="week3_item58.jpg"  />



분명 반복자를 이용한 전통적인 for문과 for-each문을 구분하여 작성하였는데 바이트 코드를 decompile하니 같은 코드로 결과가 나왔다. 

따라서 jvm이 이해하기에는 컬렉션에서는 둘의 차이가 없다는 것이다. 

<br>

배열은 다를까? 역시 똑같다.

똑같이 배열에 1000개의 원소를 담고 출력했다.

```java
int[] arr = new int[1000];
for (int i = 0; i < 1000; i++) {
    arr[i] = i;
}

for (int i = 0; i < arr.length; i++) {
    System.out.print(arr[i]);
}

for (int i : arr) {
    System.out.print(i);
}
```

decompile하면 다음과 같이 나온다.

<img src="https://github.com/gashe-soo/java-study/blob/master/img/ej_week3_item58_decompile2.JPG?raw=true" alt="week3_item58.jpg"  />

<br>

### for-each문의 이점

컬렉션을 중첩해 순회해야 한다면 전통적인 for문에서는 주의해야 한다.

```java
static Collection<Suit> suits = Arrays.asList(Suit.values());
static Collection<Rank> ranks = Arrays.asList(Rank.values());

public static void main(String[] args) {
    List<Card> deck = new ArrayList<>();
    
    
    // What's Happen?
    for (Iterator<Suit> i = suits.iterator(); i.hasNext(); )
        for (Iterator<Rank> j = ranks.iterator(); j.hasNext(); )
            deck.add(new Card(i.next(), j.next()));
    
}

enum Suit {CLUB, DIAMOND, HEART, SPADE}

enum Rank {ACE, DEUCE, THREE, FOUR, FIVE, SIX, SEVEN, EIGHT, NINE, TEN, JACK, QUEEN, KING}

static class Card {
    Suit suit;
    Rank rank;
    
    public Card(Suit suit, Rank rank) {
        this.suit = suit;
        this.rank = rank;
    }    
}
```

어떤 버그가 일어날까?

```java
for (Iterator<Suit> i = suits.iterator(); i.hasNext(); )
        for (Iterator<Rank> j = ranks.iterator(); j.hasNext(); )
            deck.add(new Card(i.next(), j.next()));
```

이 부분에서 의도대로라면 `deck`에는 총 52개의 카드가 들어가야 한다. 

하지만 `deck.add()`부분에서 `i.next()`는 의도치 않게 `rank`하나 당 불리고 있다. 그래서 `deck`에는 4개의 카드만 들어갈 것이다. 

그 이후는 `NoSuchElementException`이 발생한다. 

<br>

만약 바깥 for문의 컬렉션의 크기가 안쪽 for문 컬렉션 크기의 배수라면 예외가 발생하지 않고 종료된다. 하지만 이는 우리가 의도하지 않은대로 실행하고 종료되는 것이다.

```java
enum Face { ONE, TWO, THREE, FOUR, FIVE, SIX}

static Collection<Face> faces = Arrays.asList(Face.values());

public static void main(String[] args) {
    for(Iterator<Face> i =faces.iterator();i.hasNext();)
        for(Iterator<Face> j =faces.iterator();j.hasNext();)
            System.out.println(i.next() +" " +j.next());
}
```

우리가 의도한 것은 6*6개, 총 36개가 출력되는 것이었지만

결과는 다음과 같다.

```
ONE ONE
TWO TWO
THREE THREE
FOUR FOUR
FIVE FIVE
SIX SIX
```

<br>

그렇다면 이를 해결하기 위해서는?

변수를 하나 추가해준다. 

```java
for (Iterator<Suit> i = suits.iterator(); i.hasNext(); ){
    Suit suit = i.next();
    for (Iterator<Rank> j = ranks.iterator(); j.hasNext(); )
        deck.add(new Card(suit, j.next()));
}
```

<br>

근데 이것도 변수가 추가돼 귀찮은 일이다. 

for-each문은 이것을 간단히 해결해준다.

```java
for (Suit suit : suits)
    for (Rank rank : ranks)
        deck.add(new Card(suit, rank));
```

매우 심플해졌다.

<br>

### for-each문을 사용할 수 없는 경우

#### 파괴적인 필터링(destructive filtering)

- 컬렉션을 순회하면서 선택된 원소를 제거해야 한다면 iterator의 remove 메서드를 호출해야 한다. 

#### 변형(transforming)

- 리스트나 배열을 순회하면서 그 원소의 값 일부 혹은 전체를 교체해야 한다면 반복자나 인덱스를 사용해야 한다.
- 왜 반복자나 인덱스를 사용해야 할까?
  - 아까 decompile된 코드를 다시보자.
  
  - <img src="https://github.com/gashe-soo/java-study/blob/master/img/ej_week3_item58_decompile.JPG?raw=true?raw=true" alt="week3_item58.jpg"  />
  
  - for-each문을 쓰게 되면 `num`이라는 지역변수를 사용한다. 
  
    따라서 num을 변형하더라도 num이라는 지역변수만 변하게 될 뿐 컬렉션의 원소까지는 영향이 미치지 않는다!

#### 병렬 반복(parallel iteration)

- 여러 컬렉션을 병렬로 순회해야 한다면 각각의 반복자와 인덱스 변수를 사용해 엄격하고 명시적으로 제어해야 한다.

<br><br>