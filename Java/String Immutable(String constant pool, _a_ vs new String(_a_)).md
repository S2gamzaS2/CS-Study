# String Immutable(String constant pool, "a" vs new String("a"))
Java에서 String 객체를 생성하는 데는 두 가지 방법이 있습니다.

1. 문자열 리터럴 사용

` String b = "hello";`

2. new 키워드 사용

` String a = new String("hello"); `

---

## String 비교
String 객체 비교 시 두가지 방법을 사용할 수 있습니다.
1. **'=='** 연산자 : 객체의 주소값 비교
2. **'equals()'** 메소드 : 객체의 값 비교

```
String a = new String("hello");
String b = "hello"; 
String c = "hello";

// == , equals() 두 가지 방법으로 비교해보기
System.out.print("(1) " + a.equals(b)); ////// (1)
System.out.print("(2) " + a == b); ////// (2)
System.out.print("(3) " + b == c); ////// (3)

```

```
(1) true
(2) false
(3) true
```

(1)는 두 객체의 값인 "hello"가 동일하므로 true가 출력됩니다.
하지만 (2), (3)의 경우 a,b,c가 서로 다른 객체인데도 불구하고 겱과값이 false, true로 갈립니다.
이는 각각의 문자열 생성 방식이 다르기 때문입니다.

---

## 동작방식

**1. new 키워드**
new로 객체가 생성이 되면 해당 객체는 Heap 영역에 값이 위치하게 됩니다.
보통의 객체가 생성되는 원리와 유사합니다.

```String word = new String("hello");```

**2. String 리터럴**
문자열 리터럴을 이용하여 생성하는 방식입니다.
리터럴을 String 레퍼런스에 바로 넣어줍니다.

```String word = "hello";```
이렇게 String 객체를 생성하면 String Constant Pool 이라는 공간에 값이 생성됩니다.
명칭 그대로 문자열들을 저장해놓은 Pool이라고 볼 수 있습니다.

**String Constant Pool** 은 JVM이 관리하며, 저장방식은 해시맵을 사용하여 Pool에 동일한 문자열이 있으면 하나의 인스턴스를 유지하고 없다면 새로 추가하게 됩니다.
이러한 특성 덕분에 문자열 처리의 효율성을 크게 향상시킵니다.

다음 그림을 통해 new로 생성하는 경우와 리터럴로 생성하는 경우의 메모리에 저장되는 차이를 보여드리겠습니다. 
![image](https://github.com/user-attachments/assets/b917615e-bd50-4862-ae7b-53ee97a3104f)

- word1과 word2는 new로 생성되었기 때문에 값은 같을 수 있으나 각각 다른 객체를 참조합니다
- word3 , word4 그리고 word5는 리터럴로 생성되었기때문에 String Constant Pool에 값이 있습니다
- word3 , word4는 리터럴로 생성되었고 값이 같기때문에 Pool안에 있는 같은 객체를 참조합니다

---

## String 특성 ##
Java의 String 객체는 불변(immutable)하여 생성 후 내용을 변경할 수 없습니다.
리터럴로 생성된 String은 String Constant Pool에 저장되어 재사용되며, new 키워드로 생성 시 항상 새 객체가 힙 메모리에 생성됩니다.
불변성으로 인해 Thread-safe합니다. 여기서 Thread-safe란 여러 스레드가 동시에 같은 String 객체에 접근해도 예상치 못한 결과나 오류가 발생하지 않는다는 의미입니다. 이는 동시성 프로그래밍에서 중요한 특성입니다.
문자열 변경 시 실제로는 새 객체가 생성됩니다. 이러한 특성으로 인해 String 객체는 안전하게 공유되고 사용될 수 있습니다.

---

## 요약 ##
- String은 new키워드 , String 리터럴 두가지 방법으로 생성이 가능하다.
- 비교 방법에는 == , equals()가 있는데 각각 주소값비교, 값비교 이다.
- String 리터럴로 생성하면 String Constant Pool에 객체가 저장된다.
- String 객체는 immutable하며 Thread-safe하다.


### 참고 사이트
https://velog.io/@think2wice/Java-String-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC
