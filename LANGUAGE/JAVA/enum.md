# [Java] enum 정의와 사용법

<https://limkydev.tistory.com/50>
<https://limkydev.tistory.com/66>

## Enum class 정의

우리가 흔히 상수를 정의할 때 final static string 과 같은 방식으로 상수를 정의를합니다. 하지만 이렇게 상수를 정의해서 코딩하는 경우 다양한 문제가 발생됩니다.

따라서 이러한 문제점들을 보완하기 위해 자바 1.5버전부터 새롭게 추가된 것이 바로 "Enum" 입니다.

Enum은 열거형이라고 불리며, 서로 연관된 상수들의 집합을 의미합니다.

기존에 상수를 정의하는 방법이였던 final static string 과 같이 문자열이나 숫자들을 나타내는 기본자료형의 값을 enum을 이용해서 같은 효과를 낼 수 있습니다.

## Enum의 장점

Enum을 사용하면서 우리가 얻을 수 있는 이점은 다음과 같습니다.

1. 코드가 단순해지며, 가독성이 좋습니다.
2. 인스턴스 생성과 상속을 방지하여 상수값의 타입안정성이 보장됩니다.
3. enum class를 사용해 새로운 상수들의 타입을 정의함으로 정의한 타입이외의 타입을 가진 데이터값을 컴파일시 체크한다.
4. 키워드 enum을 사용하기 때문에 구현의 의도가 열거임을 분명하게 알 수 있습니다.

예제를 보면서 enum의 장점을 하나씩 찾아봅시다.

```java
package EnumExample;

public class EnumExample {

       //기존에 상수를 정의하는 방법
    public static final String MALE = "MALE";
    public static final String FEMALE = "FEMALE";
    public static void main (String[] args) {
        String gender1;

        gender1 = EnumExample.MALE;
        gender1 = EnumExample.FEMALE;

        //MALE, FEMALE이 아닌 상수 값이 할당 될 때
        //컴파일시 에러가 나지 않음. -> 문제점 발생.
        gender1 = "boy";

        Gender gender2;
        gender2 = Gender.MALE;
        gender2 = Gender.FEMAL;

        //컴파일 시 의도하지 않는 상수 값을 체크할 수 있음.
        //Enum으로 정의한 상수값만 할당 받을 수 있음.
        gender2 = "boy";
    }
}
//enum class를 이용해 Gender라는 새로운 상수들의 타입을 정의한다.
enum Gender {
    MALE,FEMAL;
}
```

위 코드를 분석해보겠습니다.

기존의 상수를 정의하는 방법에 따라 다음과 같이 상수를 정의했습니다.

```java
public static final String MALE = "MALE";
public static final String FEMALE = "FEMALE";
```

그리고 String gender1; 을 통해 우리가 정의한 상수타입과 같은 String 타입의 변수 gender1을 선언합니다.

static 키워드를 통해 인스턴스없이 상수를 사용할 수 있도록 MALE, FEMALE 을 정의했기 때문에

다음과 같이 gender1의 각각 원하는 상수를 할당 할 수 있습니다.

```java
gender1 = EnumExample.MALE;
gender1 = EnumExample.FEMALE;
```

여기까지는 큰 문제가 없어보입니다.

하지만 우리가 기대하는 MALE, FEMALE 의 상수가 아닌 gender1과 같은 타입이면서 엉뚱한 값이 들어간다면 이야기는 달라집니다.

즉 아래와 같이 gender1이라는 변수에 우리가 원치않는 "boy" 라는 값이 실수로 할당되었을 때 따로 우리가 기대하는 상수값이 할당되었는지 안되었는지에 대한 유효성을 검사하는 로직이 들어가 있지 않다면, 런타임시 문제가 발생할 것이고, 우리가 원치않은 상수값이 애당초 할당되었기 때문에 엉뚱한 결과값을 얻을 수 있습니다.

```java
gender1 = "boy";
```

이러한 문제가 발생하는 것은 자바 컴파일시 문제점을 발견할 수 없기 때문입니다.

자바입장에서는 String이라는 타입에 String 값이 할당되었기 때문에 이상하지 않습니다.

만약 상수가 무수히 많고 복잡하다면, 이 잠재적인 문제는 프로그램의 치명적인  오작동을 초래할 수 있습니다.

위 문제를 해결하기 위해 enum을 사용해봅시다.

예제 아래쪽 코드를 보면 enum을 통해 Gender라는 상수타입을 정의했습니다.

```java
enum Gender{   MALE,FEMAL;    }
```

그리고나서 다음과 같이 Gender 타입으로 gender2라는 변수를 선언합니다.

```java
Gender gender2;
```

gender2가 Gender타입이기 때문에 다음과 같이 Gender타입인 MALE, FEMALE을 할당 할 수 있습니다.

```java
gender2 = Gender.MALE;
gender2 = Gender.FEMAL;
```

여기서 의문점이 드셔야합니다. enum class의 인스턴스를 따로 생성하지 않고 바로 MALE, FEMALE 접근하였는데.

내부적으로 생략되었을 뿐 다음 class와 같습니다.

```java
class Gender{
public static final Gender MALE = new Gender();
public static final Gender FEMALE = new Gender();
}
```

static이 생략되었을 뿐 엄연히 enum class 안에 정의된 상수들은 static 변수 효과를 나타내는 것이죠.

이제 아까와 같은 실수로 값을 잘못 할당했을 경우를 봅시다.

```java
gender2 = "boy";
```

genfer2라는 타입은 Gender라는 상수타입인데 String 타입인 boy를 할당하려고하니 컴파일에러가 발생됩니다.
즉 컴파일시 우리가 기대하지 않는 상수값의 할당을 사전에 체크할 수 있는 것입니다.

### 정리

1. 클래스처럼 보이게 하는 상수
2. 서로 관련있는 상수들끼리 모아 상수들을 대표할 수 있는 이름으로 타입을 정의하는 것
3. Enum 클래스 형을 기반으로 한 클래스형 선언

정도로 요약할 수 있습니다.

## 1. Enum을 이용한 열거형 선언하기

EnumClass는 다른 클래스와 마찬가지로 별도의 java파일, 클래스 안, 클래스 밖 선언 가능

### 1) 별도의 .java 선언

DevType.java

```java
package EnumExample;

public enum DevType {
    MOBILE, WEB, SERVER
}
```

Developer.java

```java
package EnumExample;
public class Developer {

    public String name;
    public int career;
    public DevType type;

}
```

### 2) Class 내부에서 선언

Developer.java

```java
package EnumExample;
package EnumExample;

public class Developer {

    public String name;
    public int career;
    public enum DevType {
        MOBILE, WEB, SERVER
    }
}
```

### 3) Class 외부에서 선언

Develoer.java

```java
package EnumExample;

public class Developer {
    public String name;
    public int career;
    public DevType type;
}

enum DevType {
    MOBILE, WEB, SERVER
}
```

### * 특징

- 열거형으로 선언된 순서에 따라 0 부터 인덱스 값을 가진다. 순차적으로 증가된다.
- enum 열거형으로 지정된 상수들은 모두 대문자로 선언
- 마지막에 열거형 변수들을 선언한 후 세미콜론(;)은 찍지 않는다.(상수와 연관된 문자를 연결시킬 경우 세미콜론`(;)` 찍는다. 맨아래 예제 나와있음.)

## 2. Enum Class 사용하기

```java
package EnumExample;

public class Developer {
    public String name;
    public int career;
    public DevType type;
    public static void main(String[] args){
        Developer developer = new Developer();
        developer.name = "홍길동";
        developer.career = 3;
        developer.type = DevType.WEB;
        System.out.println("개발자 이름 : "+ developer.name);
        System.out.println("개발자 경력 : "+ developer.career);
        System.out.println("직무파트     : "+ developer.type);
    }
}
enum DevType {
    MOBILE, WEB, SERVER
}
```

### * 결과
개발자 이름  : 홍길동
개발자 경력  : 3
직무파트     :  WEB

### 3. Enum 메소드

대표적으로 values(), ordinal(), valueOf() 3가지를 예제를 들어 알아보겠습니다.

<table>

<tr><td rowspan=3>Static<br/>Methods</td><td>valueOf(String arg)</td><td>String 값을 enum에서 가져옴. 값이 없으면 예외 발생</td></tr>
<tr><td>valueOf(Class&lt;T&gt; class, String arg)</td><td>넘겨받은 class에서 String찾아, enum에 가져옴.<br/>valueOf(String arg)는 내부적으로 자기자신 Class를 가져옴.</td></tr>
 <tr><td>values()</td><td>eum의 요소들을 순서대로 enum타입의 배열로 리턴.<br/>(ENUM$VALUES)의 카피임으로 자주 호출하지 않길</td></tr>
<tr><td rowspan=4>Static이 아닌<br/>Methods</td><td>name()</td><td>호출된 값의 이름을 String으로 리턴.</td></tr>
<tr><td>ordinal()</td><td>해당 값이 enum에 정의된 순서를 리턴.</td></tr>
<tr><td>compareTo(E o)</td><td>enum과 지정된 객체의 순서를 비교. 지정된 객체보다 작은 경우<br/>음의 정수, 동일하면 0, 크면 양의 정수 리턴</td></tr>
<tr><td>equals(Object other)</td><td>지정된 객체가 enum 정수와 같은경우, true를 리턴.</td></tr>
</table>

### 1) values() : 열거된 모든 원소를 배열에 담아 순서대로 리턴

```java
package EnumExample;

public class Developer {
    public static void main(String[] args){
        for(DevType type : DevType.values()){
            System.out.println(type);
        }
    }
}
enum DevType {
    MOBILE, WEB, SERVER
}
```

### * 결과
MOBLE
WEB
SERVER

### 2) ordinal() : 원소에 열거된 순서를 정수 값으로 리턴

```java
package EnumExample;

public class Developer {

    public String name;
    public int career;
    public DevType type;
    public static void main(String[] args){
        Developer developer = new Developer();
        developer.name = "홍길동";
        developer.career = 3;
        developer.type = DevType.MOBILE;

        System.out.println(developer.type.ordinal());
        DevType tp = developer.type.SERVER;
        System.out.println(tp.ordinal());
    }
}

enum DevType {
    MOBILE, WEB, SERVER
}
```

### * 결과
0
2

### 3) valueOf() : 매개변수로 주어진 String과 열거형에서 일치하는 이름을 갖는 원소를 리턴

(일치하지 않는 경우 예외(IllegalArgumentException) 발생)

```java
package EnumExample;

public class Developer {
    public String name;
    public int career;
    public DevType type;
    public static void main(String[] args){
        DevType tp1 = DevType.MOBILE;
        DevType tp2 = DevType.valueOf("WEB");
        System.out.println(tp1);
        System.out.println(tp2);
    }
}
enum DevType {
    MOBILE, WEB, SERVER
}
```

### * 결과
MOBILE
WEB

### 원하는 EnumType가져오는 방법
*enum형 객체를 만들어서 가져오기. -> DevType tp1 = DevType.MOBILE;
*valueOf() 메소드를 이용해서 가져오기. -> DevType tp2 = DevType.valueOf("WEB");

## 4. 열거형 상수를 다른 값과 연결하기

열거형 상수와 관련된 값을 생성자를 통해 연결시킬 경우 세미콜론(;)을 붙여야 함

```java
package EnumExample;

public class Developer {
    public String name;
    public int career;
    public DevType type;
    public static void main(String[] args){
        for(DevType type : DevType.values()){
            System.out.println(type.getName());
        }
    }
}

enum DevType {
    //상수("연관시킬 문자") <- 이땐 줄 끝에 세미콜론 (;) 붙이기.
    MOBILE("안드로이드"), WEB("스프링"), SERVER("리눅스");
    final private String name;
    public String getName() {
        return name;
    }
    private DevType(String name){
        this.name = name;
    }
}
```

### * 결과
안드로이드
스프링
리눅스

자. 지금까지 enum의 사용법에 대해서 한번 알아보았습니다.
enum class가 기본적으로 제공해주는 메서드들을 이용해서 enum을 다양한 형태로 가져와서 처리할 수 있었습니다.

enum class 내부적으로 상수를 정의하는 것 이외에도 생성자를 만들고 그 인자로하여금 상수와 연관된 값을 연결시켜줌으로써 상수를 확장시킬 수 있었습니다.
또 지난번에 잠깐 언급했지만, 생성자를 private로 꼭 막아줘야 다른곳에서 newInstance를 통해 동적으로 변화시킬 수 없게끔 원천봉쇄시킬 수 있는 겁니다
