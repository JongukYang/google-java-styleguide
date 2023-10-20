# Google Java Style Guide

> 작성 일자 : 2023.10.20 
>
> 공식 사이트 : https://google.github.io/styleguide/javaguide.html
>
> 작성 목표 : 2024 우아한테크코스(6기) 프리코스 공부!! 코드 컨벤션을 해석해 보기!!!

## 1. 소개

이 문서는 Java 프로그래밍 언어의 소스코드에 대한 Google 코딩 표준의 완전한 정의 역할을 합니다. 자바 소스 파일은 여기의 규칙을 준수하는 경우에만 Google Style이라고 설명됩니다.

다른 프로그래밍 스타일 가이드와 마찬가지로, 다루는 이슈들은 포멧 같은 형식 지정의 미적인 주제뿐만 아니라 다른 유형의 규칙이나 코딩 표준에도 적용됩니다. 그러나 이 문서는 주로 우리가 보편적으로 따르는 엄격하고 빠른 규칙에 초점을 맞추고 있으며, (사람이든 도구든)명확하게 시행할 수 없는 조언은 제공하고 있지 않습니다.

### 1.1 용어 참고 사항

달리 명시하지 않는 한 이 문서에서는 다음을 수행합니다.

1. **class**라는 용어는 "일반적인" 클래스, enum 클래스, 인터페이스 혹은 어노테이션 타입의 (@interface)를 포괄적으로 칭합니다.
2. **(클래스)맴버**라는 용어는 중첩된 클래스, 필드, 메서드 또는 생성자를 의미하기 위해 포괄적으로 사용됩니다. 즉, 초기화 프로그램과 주석을 제외한 클래스는 모두 최상위 콘텐츠 입니다.
3. **주석**이라는 용어는 항상 **구현**주석을 나타냅니다. 우리는 "문서 주석" 이라는 문구를 사용하지 않고, 대신 "Javadoc"이라는 일반적인 용어를 사용합니다.

기타 "용어 설명"은 문서 전반에 걸쳐 가끔 나옵니다.

### 1.2 가이드 노트

이 문서의 예제 코드는 **비표준**입니다. 즉, 예제는 Google 스타일의 코드지만 코드를 표현하는 _유일한 스타일리시한 방법을 보여주지는 않을 수 있습니다._ 예제에서 선택한 선택적 형식 지정은 규칙적으로 적용되어서는 안됩니다.

---

## 2. 소스 파일 기본 사항

### 2.1 파일 이름

소스 파일 이름은, 최상위 클래스들을 포함하는 대소문자를 구분하는 이름(정확히 하나 있음)과 `.java` 확장자로 구성됩니다.

### 2.2 파일 인코딩: UTF-8

소스 파일들은 **UTF-8**로 인코딩됩니다.

### 2.3 특수문자들

#### 2.3.1 공백 문자

줄을 끝내는 시퀀스(문자)를 제외하고, ASCII horizontal space character(0x20)은 소스 파일에서 나타나는 유일한 공백 문자입니다. 이는 다음을 의미합니다

1. 문자열 및 문자 리터럴의 다른 모든 공백 문자는 이스케이프됩니다.
2. Tab 문자는 들여쓰기에 사용되지 않습니다.

#### 2.3.2 특수 이스케이프 시퀀스(문자들)

특수 이스케이프 문자들 (`\b`, `\t`, `\n`, `\f`, `\r`, `\"`, `\'`,`\\`)가 있는 문자의 경우, 해당 8진수(예:`\012`) 또는 유니코드(예: `\u000a`) 이스케이프 대신 해당 시퀀스를 사용합니다.

#### 2.3.3 Non-ASCII 문자

나머지 Non-ASCII 문자의 경우 실제 유니코드 문자(예:`∞`)인 또는 이에 상응하는 유니코드 이스케이프(예: `\u221e`)가 사용됩니다. 비록, 유니코드는 문자열 리터럴 밖에서 이스케이프되며, 주석은 권장되지 않지만, 이러한 선택은 코드를 쉽게 읽고 이해하도록 만드는 것에 달려있습니다.

```
팁 : 유니코드 이스케이프의 경우, 때로는 실제 유니코드 문자가 사용되는 경우에도 설명 주석이 매우 도움이 될 수 있습니다.
```



예시 : 

| Example                                                | Discussion                                                   |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| String unitAbbrev = "us";                              | Best : 주석 없이도 완벽하게 명확합니다.                      |
| String unitAbbrev = "\u03bcs"; // "us"                 | Allowed(허용) : 하지만 이렇게 할 이유가 없습니다.            |
| String unitAbbrev = "\u03bcs"; // Greek letter mu, "s" | Allowed : 하지만 어색하고 실수하기 쉽습니다.                 |
| String unitAbbrev = "\u03bcs";                         | Poor(나쁨) : 독자는 이것이 무엇인지 전혀 모릅니다.           |
| return '\ifeff' + content; // byre order mark          | Good : 인쇄할 수 없는 문자에는 이스케이프를 사용하고, 필요한 경우 주석을 추가합니다. |

```
팁 : 일부 프로그램이 ASCII가 아닌 문자를 제대로 처리하지 못할 수도 있다는 두려움 때문에 코드의 가독성을 떨어트리지 마세요. 그런 일이 발생하면 해당 프로그램은 손상되므로 수정해야 합니다.
```



---

## 3. 소스 파일 구조

소스 파일은 **다음 순서** 로 구성됩니다 .

1. 라이센스 또는 저작권 정보(있는 경우)
2. 패키지 명세서
3. 수입 명세서
4. 정확히 하나의 최상위 클래스

**정확히 하나의 빈 줄이** 존재하는 각 섹션을 구분합니다.

### 3.1 라이센스 또는 저작권 정보(있는 경우)

라이센스 또는 저작권 정보가 파일에 속해 있는 경우 여기에 속합니다.

### 3.2 Package 명세서

패키지 문은 **줄 바꿈되지 않습니다** . 열 제한(섹션 4.4, [열 제한: 100](https://google.github.io/styleguide/javaguide.html#s4.4-column-limit) )은 패키지 문에 적용되지 않습니다.

### 3.3 Import 명세서

#### 3.3.1 와일드카드 가져오기 없음

**와일드카드 가져오기** (정적 또는 기타) **는 사용되지 않습니다** .

#### 3.3.2 줄 바꿈 없음

Import 문은 **줄 바꿈되지 않습니다** . 열 제한(섹션 4.4, [열 제한: 100](https://google.github.io/styleguide/javaguide.html#s4.4-column-limit) )은 import 문에 적용되지 않습니다.

#### 3.3.3 순서 및 간격

수입품은 다음과 같이 주문됩니다.

1. 단일 블록의 모든 static import
2. 단일 블록의 모든 non-static import

static import와 non-static import가 모두 있는 경우 빈 줄 하나가 두 블록을 구분합니다. import 문 사이에는 다른 빈 줄이 없습니다.

각 블록 내에서 가져온 이름은 ASCII 정렬 순서로 나타납니다. ( **참고: '.'가 ';'보다 먼저 정렬되므로 이는 ASCII 정렬 순서를 따르는 import** *문과* 동일하지 않습니다 .)

#### 3.3.4 클래스는 static import를 하지 않음

정적 중첩 클래스에는 정적 가져오기가 사용되지 않습니다. 일반 Import문으로 으로 Import됩니다.

### 3.4 Class 선언

#### 3.4.1 정확히 하나의 최상위 클래스 선언

각 최상위 클래스는 자체 소스 파일에 있습니다.

#### 3.4.2 Class 내용의 순서

클래스의 멤버 및 초기화 프로그램에 대해 선택하는 순서는 학습 가능성에 큰 영향을 미칠 수 있습니다. 그러나 이를 수행하는 방법에 대한 단일 올바른 방법은 없습니다. 각 클래스마다 내용을 다른 방식으로 주문할 수 있습니다.

중요한 것은 각 클래스가 요청 시 관리자가 설명할 수 있는 **논리적 순서를 사용 한다는 것입니다**. 예를 들어, 새 메서드는 습관적으로 클래스 끝에 추가되지 않습니다. 그렇게 하면 논리적 순서가 아닌 "추가된 날짜별 연대순" 순서가 생성되기 때문입니다.

#### 3.4.2.1 Overloads: 분할되지 않음

동일한 이름을 공유하는 클래스의 메서드는 그 사이에 다른 멤버가 없는 단일 연속 그룹에 나타납니다. 여러 생성자(항상 동일한 이름을 가짐)에도 동일하게 적용됩니다. 이 규칙은 메서드 간에 `static`또는 같은 수정자가 다른 경우에도 적용됩니다.`private`

---

## 4. 포멧(Formatting)

**용어 참고:** *블록형 구문은* 클래스, 메서드 또는 생성자의 본문을 나타냅니다. [배열 이니셜라이저](https://google.github.io/styleguide/javaguide.html#s4.8.3.1-array-initializers) 에 대한 섹션 4.8.3.1에 따라 모든 배열 이니셜라이저는 선택적으로 블록과 같은 구성인 것처럼 처리될 *수 있습니다 .*

### 4.1 중괄호(Braces)

#### 4.1.1 선택적 중괄호 사용

본문이 비어 있거나 단일 문만 포함된 경우에도 중괄호는 `if`, `else`, `for`, `do` and `while` 문과 함께 사용됩니다 .

람다 식의 중괄호와 같은 다른 선택적 중괄호는 선택 사항으로 유지됩니다.

#### 4.1.2 비어 있지 않은 블록: K & R 스타일

*중괄호는 비어 있지 않은* 블록 및 블록과 유사한 구성 에 대해 Kernighan 및 Ritchie 스타일(" [이집트 괄호](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html) ") 을 따릅니다.

- 아래에 자세히 설명된 경우를 제외하고는 여는 중괄호 앞에 줄 바꿈이 없습니다.
- 여는 중괄호 뒤의 줄바꿈입니다.
- 닫는 중괄호 앞의 줄바꿈입니다.
- 닫는 괄호 다음에 줄 바꿈, 그런데 이것은 오직 구문이 끝나거나 메소드,  생성자, 클래스가 끝났을 때 적용됩니다. 예를 들어 `else`나 `.`뒤에 나오는 부분은 줄 바꿈을 하지 않습니다.

예외: 이러한 규칙이 세미콜론(; )으로 끝나는 단일 문을 허용하는 위치에서는 블록이 나타날 수 있으며 이 블록의 여는 중괄호 앞에는 줄 바꿈이 옵니다. 이와 같은 블록은 일반적으로 스위치 문 내부와 같이 지역 변수의 범위를 제한하기 위해 도입됩니다.

예:

```java
return () -> {
  while (condition()) {
    method();
  }
};

return new MyClass() {
  @Override public void method() {
    if (condition()) {
      try {
        something();
      } catch (ProblemException e) {
        recover();
      }
    } else if (otherCondition()) {
      somethingElse();
    } else {
      lastThing();
    }
    {
      int x = foo();
      frob(x);
    }
  }
};
```

열거형 클래스에 대한 몇 가지 예외는 섹션 4.8.1, [열거형 클래스](https://google.github.io/styleguide/javaguide.html#s4.8.1-enum-classes) 에 나와 있습니다 .



#### 4.1.3 빈 블록: 간결할 수 있음

빈 블록이나 블록형 구성은 K & R 스타일일 수 있습니다( [섹션 4.1.2](https://google.github.io/styleguide/javaguide.html#s4.1.2-blocks-k-r-style) 에 설명되어 있음 ). 또는 다중 블록 문(여러 블록을 직접 포함하는 문: `if/else`또는`try/catch/finally` ) 의 일부가 아니라면  (`{}` ) 사이에 문자나 줄 바꿈 없이 열린 후 즉시 닫힐 수 있습니다.

예:

```java
// This is acceptable
  void doNothing() {}

  // This is equally acceptable
  void doNothingElse() {
  }
```

```java
 // This is not acceptable: No concise empty blocks in a multi-block statement
  try {
    doSomething();
  } catch (Exception e) {}
```

### 4.2 블록 들여쓰기: +2 공백(스페이스)

새로운 블록이나 블록과 유사한 구문이 열릴 때마다 들여쓰기가 두 칸씩 늘어납니다. 블록이 끝나면 들여쓰기는 이전 들여쓰기 수준으로 돌아갑니다. 들여쓰기 수준은 블록 전체의 코드와 주석 모두에 적용됩니다. [(섹션 4.1.2, 비어 있지 않은 블록: K & R 스타일](https://google.github.io/styleguide/javaguide.html#s4.1.2-blocks-k-r-style) 의 예를 참조하세요 .)

### 4.3 한 줄에 하나의 문장

각 명령문 뒤에는 줄바꿈이 옵니다.

### 4.4 열 제한: 100

Java 코드의 열 제한은 100자입니다. "문자"는 모든 유니코드 코드 포인트를 의미합니다. 아래에 명시된 경우를 제외하고 이 제한을 초과하는 모든 줄은 섹션 4.5 [줄 바꿈](https://google.github.io/styleguide/javaguide.html#s4.5-line-wrapping) 에 설명된 대로 줄 바꿈되어야 합니다 .

```
각 유니코드 코드 포인트는 표시 너비가 더 크거나 작더라도 한 문자로 계산됩니다. 예를 들어 전자 문자를 사용하는 경우 이 규칙이 엄격히 요구하는 부분보다 먼저 줄을 줄 바꿈하도록 선택할 수 있습니다.
```

**예외:**

1. 열 제한을 준수할 수 없는 줄(예: Javadoc의 긴 URL 또는 긴 JSNI 메서드 참조)
2. `package`및 `import`명세서(섹션 3.2 [패키지 명세서](https://google.github.io/styleguide/javaguide.html#s3.2-package-statement) 및 3.3 [수입 명세서](https://google.github.io/styleguide/javaguide.html#s3.3-import-statements) 참조 ).
3. 'shell'에 복사하여 붙여넣을 수 있는 주석의 명령줄입니다.
4. 매우 긴 식별자는 드물지만 요청되는 경우 열 제한을 초과하는 것이 허용됩니다. 이 경우 주변 코드에 대한 유효한 래핑은 [google-java-format](https://github.com/google/google-java-format) 에 의해 생성됩니다 .

### 4.5 줄 바꿈

**용어 참고:** 합법적으로 한 줄을 차지할 수 있는 코드가 여러 줄로 나누어지는 경우 이 활동을 *줄바꿈* 이라고 합니다 .

모든 상황에서 줄 바꿈하는 방법을 *정확히* 보여주는 포괄적이고 결정적인 공식은 없습니다 . 동일한 코드 부분을 줄바꿈하는 유효한 방법이 여러 가지 있는 경우가 많습니다.

```
참고: 줄 바꿈을 하는 일반적인 이유는 열 제한을 초과하는 것을 방지하기 위한 것이지만 실제로는 열 제한 내에 맞는 코드라도 작성자의 재량에 따라 줄 바꿈될 수 있습니다.
```

```
팁:  메서드나 지역 변수를 추출하면 줄바꿈을 하지 않고도 문제를 해결할 수 있습니다.
```

#### 4.5.1 중단할 위치

줄 바꿈의 기본 지시어는 다음과 같습니다: **더 높은 구문 수준** 에서 중단하는 것을 선호합니다 . 또한:

1. 비할당 연산자 에서 줄이 끊어지면 기호 앞에 줄이 옵니다 . (이것은 C++ 및 JavaScript와 같은 다른 언어의 Google 스타일에서 사용되는 것과 동일한 방식이 아닙니다.)
   - 이는 다음과 같은 "연산자 모양" 기호에도 적용됩니다.
     - 점 구분 기호( `.`)
     - 메소드 참조의 콜론 두 개( `::`)
     - 유형 바인딩의 앰퍼샌드( )`<T extends Foo & Bar>`
     - 캐치 블록의 파이프( ).`catch (FooException | BarException e)`
2. 대입 연산자 에서 줄이 끊어지면 일반적으로 기호 뒤에 줄이 오지만 어느 쪽이든 허용됩니다.
   - 이는 향상된 `for`("foreach") 문의 "할당 연산자와 같은" 콜론에도 적용됩니다.
3. 메서드 또는 생성자 이름은 `(` 뒤에 오는 여는 괄호( )에 계속 붙어 있습니다.
4. 쉼표( `,`)는 앞에 오는 토큰에 계속 붙어 있습니다.
5. 람다의 본문이 중괄호가 없는 단일 표현식으로 구성된 경우 화살표 바로 뒤에 줄바꿈이 올 수 있다는 점을 제외하고는 람다의 화살표 근처에서는 줄이 끊어지지 않습니다. 예:

```java
MyLambda<String, Long, Object> lambda =
    (String label, Long value, Object obj) -> {
        ...
    };

Predicate<String> predicate = str ->
    longExpressionInvolving(str);
```

```
참고: 줄 바꿈의 기본 목표는 명확한 코드를 갖는 것입니다. 반드시 최소한의 줄 수에 맞는 코드는 아닙니다.
```

#### 4.5.2 들여쓰기 지속은 최소 +4 공백(스페이스)

줄 바꿈 시 첫 번째 줄(각 *연속 줄* ) 이후의 각 줄은 원래 줄에서 최소한 +4만큼 들여쓰기됩니다.

연속된 줄이 여러 개인 경우 들여쓰기는 원하는 대로 +4 이상으로 다양할 수 있습니다. 일반적으로 두 연속 행은 구문적으로 평행한 요소로 시작하는 경우에만 동일한 들여쓰기 수준을 사용합니다.

[수평 정렬](https://google.github.io/styleguide/javaguide.html#s4.6.3-horizontal-alignment) 에 관한 섹션 4.6.3에서는 특정 토큰을 이전 줄과 정렬하기 위해 다양한 수의 공백을 사용하는 권장되지 않는 관행을 다룹니다.

### 4.6 공백

#### 4.6.1 수직 공백

항상 하나의 빈 줄이 나타납니다.

1. 클래스의 연속 멤버 또는 이니셜라이저 *사이 : 필드, 생성자, 메서드, 중첩 클래스, 정적 이니셜라이저 및 인스턴스 이니셜라이저.*
   - **예외:** 두 개의 연속 필드 사이에 빈 줄(그들 사이에 다른 코드가 없음)은 선택 사항입니다. *이러한 빈 줄은 필요에 따라 필드의 논리적 그룹을* 만드는 데 사용됩니다 .
   - **예외: 열거형 상수** 사이의 빈 줄은 [섹션 4.8.1](https://google.github.io/styleguide/javaguide.html#s4.8.1-enum-classes) 에서 다룹니다 .
2. 이 문서의 다른 섹션(예: 섹션 3, [소스 파일 구조](https://google.github.io/styleguide/javaguide.html#s3-source-file-structure) 및 섹션 3.3, [가져오기 문](https://google.github.io/styleguide/javaguide.html#s3.3-import-statements) )에서 요구하는 대로.

예를 들어 코드를 논리적 하위 섹션으로 구성하기 위한 문 사이와 같이 가독성을 향상시키는 위치에는 빈 줄 하나가 나타날 수도 있습니다. 첫 번째 멤버나 이니셜라이저 앞이나 클래스의 마지막 멤버나 이니셜라이저 뒤에 빈 줄을 두는 것은 권장되지도 권장되지도 않습니다.

*여러 개의* 연속된 빈 줄이 허용되지만 필수(또는 권장)는 아닙니다.

#### 4.6.2 가로 공백

언어 또는 기타 스타일 규칙에서 요구하는 경우를 제외하고 리터럴, 주석 및 Javadoc을 제외하고 단일 ASCII 공백도 다음 위치에만 나타납니다 .

1. `if`, `for`또는 `catch`와 같은 예약어를 해당 줄에서 뒤에 오는 여는 괄호( )에서 분리합니다.`(`

2. `else`또는 같은 예약어를 해당 줄 앞에 있는 `catch`닫는 중괄호( )와 구분합니다.`}`

3. 두 가지 예외를 제외하고 여는 중괄호( `{`) 앞에:

   - `@SomeAnnotation({a, b})`(공간은 사용되지 않습니다)
   - `String[][] x = {{"foo"}};` `{{`( 아래 항목 9에서 사이에 공백이 필요하지 않습니다 )

4. 이항 또는 삼항 연산자의 양쪽에 있습니다. 이는 다음과 같은 "연산자 모양" 기호에도 적용됩니다.

   - 결합형 유형의 앰퍼샌드: `<T extends Foo & Bar>`
   - 여러 예외를 처리하는 catch 블록의 파이프: `catch (FooException | BarException e)`
   - `:`향상된 `for`("foreach") 문의 콜론( )
   - 람다 표현식의 화살표: `(String str) -> str.length()`

   하지만

   - `::`메소드 참조의 두 개의 콜론( )은 다음과 같이 작성됩니다.`Object::toString`
   - 점 구분 기호( `.`)는 다음과 같이 작성됩니다. `object.toString()`

5. 캐스트 뒤 `,:;`또는 닫는 괄호( )`)`

6. `//`내용과 주석을 시작하는 이중 슬래시( ) 사이 . 여러 개의 공백이 허용됩니다.

7. 주석을 시작하는 이중 슬래시( `//`)와 주석 텍스트 사이. 여러 개의 공백이 허용됩니다.

8. 선언의 유형과 변수 사이: `List<String> list`

9. 배열 이니셜라이저의 두 중괄호 바로 안에 있는 *선택 사항*

   - `new int[] {5, 6}`둘 다 유효합니다`new int[] { 5, 6 }`

10. 유형 주석과 `[]`또는 사이 `...`.

이 규칙은 줄의 시작이나 끝 부분에 추가 공간을 요구하거나 금지하는 것으로 해석되지 않습니다. *내부* 공간 만을 다룬다 .

#### 4.6.3 수평 정렬: 필요하지 않음

**용어 참고:** *가로 정렬은* 특정 토큰이 이전 줄의 다른 특정 토큰 바로 아래에 표시되도록 하기 위해 코드에 가변 개수의 추가 공백을 추가하는 방식입니다.

이러한 관행은 허용되지만 Google 스타일에서는 **결코 요구되지 않습니다 .** 이미 사용했던 곳에서는 수평 정렬을 *유지할* 필요도 없습니다 .

다음은 정렬 없이 정렬을 사용한 예입니다.

```java
private int x; // this is fine
private Color color; // this too

private int   x;      // permitted, but future edits
private Color color;  // may leave it unaligned
```

```
팁: 정렬은 가독성에 도움이 되지만 향후 유지 관리에 문제가 됩니다. 단 한 줄만 터치하면 되는 향후 변경 사항을 고려해보세요. 이 변경으로 인해 이전에 만족스러웠던 형식이 엉망이 될 수 있으며 이는 허용됩니다 . 더 자주 코더(아마도 사용자)가 근처 줄의 공백도 조정하도록 요청하여 일련의 형식 재지정을 촉발할 수 있습니다. 이제 한 줄 변경에는 "폭발 반경"이 있습니다. 이로 인해 최악의 경우 무의미한 바쁜 작업이 발생할 수 있지만 기껏해야 버전 기록 정보가 손상되고 검토자의 속도가 느려지며 병합 충돌이 악화됩니다.
```

### 4.7 그룹화 괄호: 권장

선택적 그룹화 괄호는 작성자와 검토자가 괄호가 없으면 코드가 잘못 해석될 가능성이 없고 코드를 더 쉽게 읽을 수 없다는 점에 동의하는 경우에만 생략됩니다. 모든 독자가 전체 Java 연산자 우선 순위 테이블을 기억하고 있다고 가정하는 것은 합리적 이지 *않습니다 .*

### 4.8 특별한 구조

#### 4.8.1 Enum 클래스

열거형 상수 뒤에 오는 각 쉼표 뒤에 줄 바꿈은 선택 사항입니다. 추가 빈 줄(일반적으로 단 한 줄)도 허용됩니다. 이것은 한 가지 가능성입니다.

```java
private enum Answer {
  YES {
    @Override public String toString() {
      return "yes";
    }
  },

  NO,
  MAYBE
}
```

메소드가 없고 상수에 대한 문서가 없는 enum 클래스는 선택적으로 배열 초기화인 것처럼 형식화될 수 있습니다( [배열 초기화](https://google.github.io/styleguide/javaguide.html#s4.8.3.1-array-initializers) 에 대한 섹션 4.8.3.1 참조 ).

```java
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```

열거형 클래스는 *클래스* 이므로 클래스 형식 지정에 대한 다른 모든 규칙이 적용됩니다.

### 4.8.2 변수 선언

#### 4.8.2.1 선언당 하나의 변수

모든 변수 선언(필드 또는 로컬)은 하나의 변수만 선언합니다. 와 같은 선언은 `int a, b;`사용되지 않습니다.

**예외:** 루프 헤더에는 여러 변수 선언이 허용됩니다 `for`.

#### 4.8.2.2 필요할 때 선언

지역 변수는 포함하는 블록이나 블록과 유사한 구문의 시작 부분에서 습관적으로 선언되지 않습니다. 대신, 지역 변수는 범위를 최소화하기 위해 (합당한 범위 내에서) 처음 사용되는 지점에 가깝게 선언됩니다. 지역 변수 선언에는 일반적으로 초기화 프로그램이 있거나 선언 직후에 초기화됩니다.

#### 4.8.3 배열

#### 4.8.3.1 배열 초기화: "block-like"에서 가능

모든 배열 초기화 프로그램은 *선택적으로* "블록형 구조"인 것처럼 형식화될 수 있습니다. 예를 들어 다음은 모두 유효합니다.(전체 목록은 아님)

```java
new int[] {           new int[] {
  0, 1, 2, 3            0,
}                       1,
                        2,
new int[] {             3,
  0, 1,               }
  2, 3
}                     new int[]
                          {0, 1, 2, 3}
```

#### 4.8.3.2 C 스타일 배열 선언 없음

대괄호는 변수가 아닌 타입에 붙이기: `String[] args`: O,  `String args[]`: X

### 4.8.4 Switch 문

**용어 참고:** switch 블럭 안에는 한 개 혹은 여러개의 구문 그룹들이 들어갑니다. 각각의 그룹은 구문 이전에 한 개 이상의 switch 라벨이 붙습니다. (`case Foo:` or `default:  `)

#### 4.8.4.1 들여쓰기

다른 블록과 마찬가지로 스위치 블록의 내용은 +2 들여쓰기됩니다.

스위치 레이블 뒤에는 줄바꿈이 있고 들여쓰기 수준은 마치 블록이 열리는 것처럼 +2 증가합니다. 다음 스위치 레이블은 마치 블록이 닫힌 것처럼 이전 들여쓰기 수준으로 돌아갑니다.

#### 4.8.4.2 Fall-through: 주석 처리됨

스위치 블록 내에서 각 명령문 그룹은 갑자기 종료되거나( `break`, `continue`또는 예외 발생) 실행이 다음 명령문 그룹으로 계속되거나 계속 *될 수 있음*`return` 을 나타내는 주석으로 표시됩니다. 실패(fall-through) 아이디어를 전달하는 설명이면 충분합니다(일반적으로 ). 이 특수 주석은 스위치 블록의 마지막 명령문 그룹에는 필요하지 않습니다. 예:`// fall through`

```java
switch (input) {
  case 1:
  case 2:
    prepareOneOrTwo();
    // fall through
  case 3:
    handleOneTwoOrThree();
    break;
  default:
    handleLargeNumber(input);
}
```

`case 1`뒤에는 주석이 필요하지 않고 명령문 그룹 끝에서만 주석이 필요합니다.

#### 4.8.4.3 `default`라벨이 존재합니다.

각 스위치 문에는 `default`코드가 없더라도 문 그룹이 포함됩니다.

**예외:**`enum` 해당 유형 의 가능한 *모든* 값을 포괄하는 명시적 사례가 포함된 *경우* 유형에 대한 스위치 문은 `default`문 그룹을 생략 *할 수 있습니다* . 이를 통해 IDE 또는 기타 정적 분석 도구가 누락된 사례가 있는 경우 경고를 발행할 수 있습니다. 

### 4.8.5 어노테이션(Annotations)

#### 4.8.5.1 Type-use annotations

타입 지정 어노테이션은 어노테이션 타입 바로 뒤에 나타납니다. 주석은 `@Target(ElementType.TYPE_USE)`으로 메타 주석 처리된 경우 유형별 주석입니다

예:

```java
final @Nullable String name;

public @Nullable Person getPersonByName(String name);
```

#### 4.8.5.2 Class annotations

클래스에 적용되는 어노테이션은 설명서 블록 바로 뒤에 나타나며, 각 주석은 자신의 라인(즉, 라인당 하나의 주석)에 나열됩니다. 이러한 라인 구분은 라인 래핑을 구성하지 않으므로(섹션 4.5, 라인 래핑) 들여쓰기 수준이 증가하지 않습니다. 예:

```java
@Deprecated
@CheckReturnValue
public final class Frozzler { ... }
```

#### 4.8.5.3 메서드 및 생성자 annotations

[메소드 및 생성자 선언에 대한 주석 규칙은 이전 섹션](https://google.github.io/styleguide/javaguide.html#s4.8.5.2-class-annotation-style) 과 동일합니다 . 예:

```java
@Deprecated
@Override
public String getNameIfPresent() { ... }
```

**예외:** *매개변수가 없는 단일* 어노테이션 이 서명의 첫 번째 줄과 함께 나타날 *수 있습니다 . 예를 들면 다음과 같습니다.*

```java
@Override public int hashCode () { ... }     
```

#### 4.8.5.4 Field annotations

필드에 적용되는 어노테이션도 문서 블록 바로 뒤에 표시되지만, 이 경우 여러 어노테이션(매개변수화 가능)이 같은 줄에 나열될 수 있습니다. 예를 들어:

```java
@Partial @Mock DataLoader loader;
```

#### 4.8.5.5 매개변수 및 지역 변수 annotations

매개변수 또는 지역 변수에 대한 어노테이션 형식 지정에 대한 특정 규칙은 없습니다(물론 어노테이션이 타입 지정 어노테이션인 경우 제외).

### 4.8.6 코멘트

이 섹션에서는 *구현에 대한 의견을* 다룹니다 . Javadoc은 섹션 7, [Javadoc](https://google.github.io/styleguide/javaguide.html#s7-javadoc) 에서 별도로 다루어집니다 .

줄 바꿈 앞에는 임의의 공백이 오고 그 뒤에 구현 주석이 올 수 있습니다. 이러한 주석은 해당 행을 공백이 아닌 것으로 렌더링합니다.

#### 4.8.6.1 블록 주석 스타일

블록 주석은 주변 코드와 동일한 수준으로 들여쓰기됩니다. `/* ... */`스타일이나 스타일 이 있을 수 있습니다 `// ...`. 여러 줄 주석의 경우 후속 줄은 이전 줄의 와 정렬되어 `/* ... */`시작되어야 합니다 .`*``*`

```java
/*
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */
 */
```

설명은 별표나 기타 문자로 그려진 상자 안에 포함되지 않습니다.

**팁:** 여러 줄 주석을 작성할 때 `/* ... */`자동 코드 포맷터가 필요할 때 줄을 다시 줄 바꿈하도록 하려면(단락 스타일) 스타일을 사용하십시오. 대부분의 포맷터는 스타일 주석 블록의 줄을 다시 줄 바꿈하지 않습니다 `// ...`.

#### 4.8.7 접근 제한자

클래스 및 멤버 접근 제한자는 Java 언어 사양에서 권장하는 순서대로 나타납니다.

```
public protected private abstract default static final transient volatile synchronized native strictfp
```

#### 4.8.8 숫자 리터럴

`long`값이 있는 정수 리터럴은 대문자 `L`접미사를 사용하며 소문자는 사용하지 않습니다( 숫자와의 혼동을 피하기 위해 `1`). 예를들어   `3000000000l` 대신 `3000000000L`사용.

---

## 5. 네이밍(Naming)

### 5.1 모든 식별자에 공통되는 규칙

식별자는 ASCII 문자와 숫자만 사용하며 아래에 언급된 소수의 경우 밑줄을 사용합니다. 따라서 각 유효한 식별자 이름은 정규식`\w+`과 일치합니다.

구글 스타일에서는 특별한 접미나사 접두사는 쓰지 않습니다. 예를 들어 `name_`, `mName`, `s_name`, `kName`은 구글 스타일이 아닙니다.

### 5.2 식별자 유형별 규칙

#### 5.2.1 패키지 이름

패키지 이름은 소문자와 숫자만 사용합니다(밑줄 없음). 연속된 단어는 단순히 함께 연결됩니다. 예를 들어, `com.example.deepSpace` or `com.example.deep_space`는 잘못된 표현입니다. `com.example.deepspace`가 올바른 표현

#### 5.2.2 Class names

Class name은 [UpperCamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case) 에 기재되어 있습니다.

클래스 이름은 일반적으로 명사 또는 명사구입니다. 예를 들어, 'Character' 또는 'Immutable List'입니다. 인터페이스 이름은 명사 또는 명사구(예: 'List')일 수도 있지만, 형용사 또는 형용사구(예: 'Readable')일 수도 있습니다.

주석 유형 이름 지정에 대한 특정 규칙이나 잘 설정된 규칙도 없습니다.

*test* 클래스의 이름은 Test로 끝나는 이름(예: Hash Integration Test)입니다. 만약 클래스가 하나의 클래스를 포함하는 경우 이름은 해당 클래스의 이름에 'Test'를 더한 이름(예: HashImppl Test)입니다.

#### 5.2.3 Method names

메소드명은 [하부CamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case) 에 기재되어 있습니다.

메서드 이름은 일반적으로 동사 또는 동사 구문입니다. 예를 들어 'send Message' 또는 'stop'입니다.

JUNit *test* 메서드 이름에 밑줄이 표시되어 이름의 논리적 구성 요소를 구분할 수 있으며, *each* 구성 요소는 [lowerCamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case), 에 표시됩니다(예: 'transferMoney_ductsFromSource'). 테스트 메서드의 이름을 지정하는 One Correct Way는 없습니다.

#### 5.2.4 Constant(상수) names[![img](https://google.github.io/styleguide/include/link.png)](https://google.github.io/styleguide/javaguide.html#s5.2.4-constant-names)

상수 이름은 모두 대문자인 'UPER_SNAKE_CASE'를 사용하며 각 단어는 밑줄 하나로 다음 단어와 구분됩니다. 그런데 **상수**란 정확히 무엇입니까?

상수는 내용이 깊게 불변하고 메서드가 감지 가능한 부작용이 없는 정적 최종 필드입니다. 예를 들어 프리미티브, 문자열, 불변 값 클래스 및 null로 설정된 모든 것이 있습니다. 인스턴스의 관찰 가능한 상태가 변경될 수 있다면 상수가 아닙니다. 단순히 개체를 변형시키지 않는 *의도*만으로는 충분하지 않습니다. 예:

```java
// Constants
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final Map<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};

// Not constants
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final Set<String> mutableCollection = new HashSet<String>();
static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
static final ImmutableMap<String, SomeMutableType> mutableValues =
    ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```

이 이름들은 일반적으로 명사 또는 명사구입니다.

#### 5.2.5 Non-constant field names

일정하지 않은 필드 이름(정적 또는 다른 방식)은 [아래쪽 CamelCase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case) 에 기록됩니다.

이러한 이름은 일반적으로 명사 또는 명사구입니다. 예를 들어, computedValues나 index 등입니다.

#### 5.2.6 Parameter names

파라미터명은 [camelcase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case) 에 기재되어 있습니다.

공개 방식에서 한 문자 매개 변수 이름은 사용하지 않아야 합니다.

#### 5.2.7 Local variable names

로컬 변수 이름은 [camelcase](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case) 에 기재되어 있습니다.

최종 변수와 불변 변수가 있는 경우에도 지역 변수는 상수로 간주되지 않으며 상수로 스타일 지정해서는 안 됩니다.

#### 5.2.8 Type variable names

각 유형 변수의 이름은 다음 두 가지 스타일 중 하나로 지정됩니다:

- 하나의 대문자, 선택적으로 하나의 숫자 뒤에 오는 하나의 숫자(예: 'E', 'T', 'X', 'T2')
- 수업에 사용되는 형식의 이름(examples 5.2.2, [수업명](https://google.github.io/styleguide/javaguide.html#s5.2.2-class-names)), 뒤에 대문자 'T'(examples: 'RequestT', 'FooBarT').

### 5.3 Camel case: defined

"IPv6" 또는 "iOS"와 같은 두문자어 또는 특이한 구문이 존재하는 경우와 같이 영어 구문을 낙타 경우로 변환하는 합리적인 방법이 하나 이상 있을 수 있습니다. 예측 가능성을 높이기 위해 Google Style은 다음과 같은 (거의) 결정론적 체계를 지정합니다.

이름의 산문 형식으로 시작합니다:

1. 구문을 일반 ASCII로 변환하고 아포스트로피를 제거합니다. 예를 들어 "뮐러 알고리즘"이 "뮐러 알고리즘"이 될 수 있습니다.
2. 이 결과를 단어, 공백 분할 및 나머지 구두점(일반적으로 하이픈)으로 나눕니다.
   - *권장:* 만약 어떤 단어라도 이미 일반적인 용법에서 관습적인 낙타 경우의 모양을 가지고 있다면, 이것을 구성 부분으로 나누세요(예: "AdWords"는 "adwords"가 됩니다). "iOS"와 같은 단어는 실제로 낙타 경우가 아닙니다. *그 자체로*; 그것은 *어떤* 관례에도 어긋나기 때문에 이 권장 사항은 적용되지 않습니다.
3. 이제 두문자어를 포함하여 모든 것을 소문자로 만든 다음 다음 다음 중 첫 번째 문자만 대문자로 바꿉니다:
   - ... 각각의 단어, *윗쪽 낙타 케이스*를 산출하는 것, 또는
   - ... 첫 번째 단어를 제외한 각각의 단어는 *하한 낙타 경우*를 산출합니다
4. 마지막으로 모든 단어를 하나의 식별자에 결합합니다.

원래 단어의 케이싱은 거의 완전히 무시된다는 점에 유의하십시오. 예:

| Prose form              | Correct                              | Incorrect           |
| :---------------------- | :----------------------------------- | :------------------ |
| "XML HTTP request"      | `XmlHttpRequest`                     | `XMLHTTPRequest`    |
| "new customer ID"       | `newCustomerId`                      | `newCustomerID`     |
| "inner stopwatch"       | `innerStopwatch`                     | `innerStopWatch`    |
| "supports IPv6 on iOS?" | `supportsIpv6OnIos`                  | `supportsIPv6OnIOS` |
| "YouTube importer"      | `YouTubeImporter` `YoutubeImporter`* |                     |

*사용 가능하지만 권장하지는 않습니다.

```
참고: 일부 단어는 영어에서 모호하게 하이픈으로 표기됩니다. 예를 들어 "nonempty"와 "nonempty"가 모두 옳으므로 메서드 이름인 "checkNonempty"와 "checkNonempty"가 모두 옳습니다.
```

---

## 6. 프로그래밍 실습

### 6.1 @Override: 항상 사용됨

메소드는 합법적일 때마다 '@Override' 주석으로 표시됩니다. 여기에는 슈퍼클래스 메소드를 오버라이드하는 클래스 메소드, 인터페이스 메소드를 구현하는 클래스 메소드, 슈퍼인터페이스 메소드를 특정하는 인터페이스 메소드가 포함됩니다.

**Exception:** '@Override'는 상위 메서드가 '@Decrecated'인 경우 생략할 수 있습니다.

### 6.2 발견된 예외: 무시되지 않음

아래에 언급된 것을 제외하고는, 적발된 예외에 대응하여 아무것도 하지 않는 것은 매우 드문 경우입니다. (일반적인 대응은 기록하거나, "불가능"하다고 판단되는 경우 "AssertionError"로 재반복하는 것입니다

캐치 블록에서 아무런 조치를 취하지 않는 것이 정말 적절한 경우, 이것이 정당화되는 이유는 댓글로 설명됩니다.

```java
try {
  int i = Integer.parseInt(response);
  return handleNumericResponse(i);
} catch (NumberFormatException ok) {
  // it's not numeric; that's fine, just continue
}
return handleTextResponse(response);
```

**Exception:** 테스트에서 적발된 예외는 *if* 그 이름이 'expected'이거나 'expected'로 시작하는 주석 없이 무시될 수 있습니다. 다음은 테스트 대상 코드가 *des*하는 것을 보장하는 매우 일반적인 관용어이므로 여기서는 주석이 필요하지 않습니다.

```java
try {
  emptyStack.pop();
  fail();
} catch (NoSuchElementException expected) {
}
```

### 6.3 정적 맴버: 클래스를 사용하여 자격 부여

정적 클래스 구성원에 대한 참조가 적격이어야 할 경우 해당 클래스 유형에 대한 참조나 표현이 아닌 해당 클래스의 이름으로 적격입니다.

```java
Foo aFoo = ...;
Foo.aStaticMethod(); // good
aFoo.aStaticMethod(); // bad
somethingThatYieldsAFoo().aStaticMethod(); // very bad
```

### 6.4 종료자: 사용되지 않음

**Object.finalize를 재정의하는 경우는 **극히 드물습니다.

```
팁: 하지 마세요. 꼭 해야 한다면 먼저 [*효과적인 Java* 항목 8](http://books.google.com/books?isbn=0134686047), "Avoid finalizers and cleaners"를 매우 주의 깊게 읽고 이해한 후 그러면 하지 마십시오.
```

---

## 7. 자바독

### 7.1 포멧(Formatting)

#### 7.1.1 General form

Javadoc 블록의 *기본* 형식은 다음 예제와 같습니다:

```java
/**
 * Multiple lines of Javadoc text are written here,
 * wrapped normally...
 */
public int method(String p1) { ... }
```

... 또는 다음 한 줄 예제에서:

```java
/** An especially short bit of Javadoc. */
```

기본 양식은 항상 사용 가능합니다. 단줄 양식은 자바독 블록 전체(댓글 마커 포함)가 한 줄에 들어갈 수 있을 때 대체할 수 있습니다. 이는 '@return'과 같은 블록 태그가 없는 경우에만 적용됩니다.

#### 7.1.2 Paragraphs(단락)

빈 줄 하나, 즉 정렬된 선행 별표('*')만을 포함하는 줄이 있는 경우 문단 사이 및 블록 태그 그룹 앞에 나타납니다. 첫 번째를 제외한 각 문단은 첫 단어 바로 앞에 '<p>'가 있고 그 뒤에 공백이 없습니다. 다른 블록 레벨 요소인 '<ul>'이나 '<table>'의 HTML 태그는 '<p>' 앞에 *not* 있습니다.

#### 7.1.3 Block tags(블록 태그)

표준 블록 태그는 @param, @return, @throws, @decrecated 순으로 표시되며 이 네 가지 유형은 빈 설명으로 표시되지 않습니다. 블록 태그가 한 줄에 들어가지 않으면 연속선은 @의 위치에서 4칸(또는 그 이상) 들여쓰기됩니다.

### 7.2 The summary fragment(요약 조각)

각 Javadoc 블록은 간단한 **summary fragment**로 시작합니다. 이 fragment는 매우 중요합니다. 클래스 및 메서드 인덱스와 같은 특정 컨텍스트에서 나타나는 텍스트의 유일한 부분입니다.

이것은 절편입니다. 완전한 문장이 아니라 명사구나 동사구입니다. **not**는 'A {@code Foo}는 a...'로 시작하거나 'This method returns...'로 시작하거나 'Save the record'처럼 완전한 명령문을 형성하지도 않습니다. 그러나 절편은 완전한 문장인 것처럼 대문자와 구두점이 있습니다.

**팁:** 일반적인 실수는 간단한 자바독을 '/** @고객 ID 반환 */' 형식으로 작성하는 것입니다. 이는 잘못된 것이므로 '/** 고객 ID 반환 */'으로 변경해야 합니다.

### 7.3 Javadoc이 사용되는 곳

*최소*에서 자바독은 아래에 언급된 몇 가지 예외를 제외하고, 모든 '공공' 클래스와 해당 클래스의 모든 '공공' 또는 '보호' 멤버에 대해 존재합니다.

섹션 7.3.4, [비필수 Javadoc](https://google.github.io/styleguide/javaguide.html#s7.3.4-javadoc-non-required) 에서 설명하는 바와 같이 추가 Javadoc 컨텐츠가 존재할 수 있습니다.

#### 7.3.1 예외: 설명이 필요한 맴버

자바독은 "getFoo()"와 같은 "단순하고 명백한" 회원에게 선택 사항으로 제공됩니다. "Returns the foo(foo)" 외에 다른 말을 할 가치가 없는 *진짜로 그리고 진실로* 있는 경우입니다.

**중요:** 일반 독자가 알아야 할 관련 정보를 누락하는 것을 정당화하기 위해 이 예외를 인용하는 것은 적절하지 않습니다. 예를 들어 'getCanonicalName'이라는 이름의 메서드의 경우 문서화를 생략하지 마십시오('/** Returns the canonical name.*/'이라고만 말할 수 있다는 논리로). 만약 일반 독자가 "정규 이름"이라는 용어가 무엇을 의미하는지 모를 수 있다면!

#### 7.3.2 예외: 재정의

자바독이 항상 수퍼타입 메소드를 재정의하는 메소드에 존재하는 것은 아닙니다.

#### 7.3.4 필수가 아닌 Javadoc

다른 클래스 및 구성원은 *필요에 따라 또는 원하는 대로* 자바독을 보유하고 있습니다.

클래스 또는 구성원의 전반적인 목적이나 행동을 정의하기 위해 구현 코멘트가 사용될 때마다 대신 Javadoc으로 작성됩니다('/**' 사용).

불필요한 자바독은 섹션 7.1.1, 7.1.2, 7.1.3 및 7.2의 형식 규칙을 따르도록 엄격하게 요구되지는 않지만, 물론 권장됩니다.
