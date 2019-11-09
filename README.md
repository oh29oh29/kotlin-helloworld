# 코틀린 Hello world!

### Hello, world

- 함수를 선언할 때 fun 키워드를 사용한다.
- 파라미터 이름 뒤에 타입을 쓴다.
- 함수를 최상위 레벨에 정의할 수 있다. (꼭 클래스 안에 함수를 넣을 필요가 없다)
- 배열도 일반적인 클래스와 마찬가지다. (배열 처리를 위한 문법이 따로 존재하지 않는다)
- System.out.println 대신 println 이라고 쓴다. 코틀린 표준 라이브러리는 여러 가지 자바 표준 라이브러리 함수를 쉽게 사용할 수 있게 wrapper를 제공한다.
- 줄 끝에 세미콜론을 붙이지 않아도 좋다.

### 함수

##### 함수 정의
```kotlin
fun max(a: Int, b: Int) : Int {
    return if (a > b) a else b
}
```

- 루프를 제외한 대부분의 제어 구조가 식이다.
- 대입문은 문이다.

##### 식이 본문인 함수
```kotlin
fun max(a: Int, b: Int) : Int = if (a > b) a else b
``` 
- 식이 본문인 함수의 경우 굳이 사용자가 반환 타입을 적지 않아도 컴파일러가 함수 본문 식을 분석해 식의 결과 타입을 함수 변환 타입으로 정해준다. (타입 추론)
- 식이 본문인 함수의 반환 타입만 생략 가능하다.

### 변수

- 코틀린에서 타입 지정을 생략하는 경우가 흔하다.
- 타입으로 변수 선언을 시작하면 타입을 생략할 경우 식과 변수 선언을 구별할 수 없기 때문에 변수 이름 뒤에 타입을 명시하거나 생략하게 허용한다.

##### 타입 명시

```kotlin
val number: Int = 1
```

##### 타입 생략

```kotlin
val number = 1
```

##### 타입과 초기화

- 초기화 식을 사용하지 않고 변수를 선언하려면 변수 타입을 반드시 명시해야 한다.
 
```kotlin
val number: Int
number = 1
```

##### 변경 가능한 변수 / 변경 불가능한 변수

- val
    - 변경 불가능한 참조를 저장하는 변수
    - 초기화하고 나면 변경이 불가능하다
    - 블록을 실행할 때 정확히 한 번만 초기화돼야 한다. 하지만 어떤 블록이 실행될 때 오직 한 초기화 문장만 실행됨을 컴파일러가 확인할 수 있다면 조건에 따라 val 값을 다른 여러 값으로 초기화할 수 있다.
- var
    - 변경 가능한 참조를 저장하는 변수
 
 ##### 문자열 템플릿
 
 ```kotlin
fun main(args: Array<String>) {
    val name = if (args.size > 0) args[0] else "Kotlin"
    println("Hello, $name!")
}
```

- 복잡한 식도 중괄호({})로 감싸서 문자열 템플릿 안에 넣을 수 있다.
```kotlin
fun main(args: Array<String>) {
    if (args.size > 0) {
        println("Hello, ${args[0]}!")
    }
}
```

### 클래스

```kotlin
class Person(val name: String)
```
- 코틀린의 기본 가시성은 public이므로 생략할 수 있다.
 
### 프로퍼티

```kotlin
class Person(
	val name: String,		// 읽기 전용 프로퍼티
	var isMarried: Boolean	// 변경 가능한 프로퍼티
)
```

- 기본적으로 코틀린에서 프로퍼티를 선언하는 방식은 프로퍼티와 관련 있는 접근자를 선언하는 것이다.
- 읽기 전용 프로퍼티의 경우, getter만 선언 / 변경 가능한 프로퍼티의 경우, getter와 setter를 모두 선언

```kotlin
val person = Person("Hyukjae", false)
println(person.name)	// Hyukjae
person.isMarried = true
println(person.isMarried)	// true
```

### 커스텀 접근자

```kotlin
class Rectangle(val height: Int, val width: Int) {
	val isSquare: Boolean
		get() {
			return height == width
		}
}
```

또는

```kotlin
class Rectangle(val height: Int, val width: Int) {
	val isSquare: Boolean
		get() = height == width
}
```

- 클라이언트가 프로퍼티에 접근할 때마다 getter가 프로퍼티 값을 매번 계산한다.

```kotlin
val rectangle = Rectangle(10, 20)
println(rectangle.isSquare)		// false
```

- 파라미터가 없는 함수를 정의하는 방식과 커스텀 getter를 정의하는 방식의 차이는 가독성 뿐이다. (성능 차이 없음)

### enum 클래스

```kotlin
enum class Color {
	RED, GREEN, BLUE
}
```

- 코틀린에서 enum은 soft keyword라 부르는 존재다.
- enum은 class 앞에 있을 때만 특별한 의미를 지닌다. 다른 부분에서는 이름에 사용할 수 있다.

```kotlin
enum class Color (
	// 상수의 프로퍼티 정의
	val r: Int, 
	val g: Int, 
	val b: Int
) {
	RED(255, 0, 0),
	GREEN(0, 255, 0),
	BLUE(0, 0, 255);
	
	fun rgb() = (r * 256 + g) * 256 + b
}
```
```kotlin
println(Color.BLUE.rgb())		// 255
```

- enum 클래스 안에 메소드를 정의하는 경우 반드시 상수 목록과 메소드 정의 사이에 세미콜론을 넣어야 한다.


### when으로 enum 클래스 다루기

- switch에 해당하는 코틀린 구성 요소는 when이다.
- if와 마찬가지로 값을 만들어내는 식이다.

```kotlin
fun getMnemonic(color: Color) =
	when (color) {
		Color.RED -> "Richard"
		Color.GREEN -> "Gave"
		Color.BLUE -> "Battle"
	}
```
```kotlin
println(getMnemonic(Color.BLUE))		// Battle
```

- 한 분기 안에서 여러 값을 매치 패턴으로 사용할 수도 있다. 값 사이를 콤마(,)로 분리한다.

```kotlin
fun getWarmth(color: Color) = 
	when(color) {
		Color.RED, Color.ORANGE, Color.YELLOW -> "warm"
		Color.GREEN -> "neutral"
		Color.BLUE, Color.VIOLET -> "cold"
	}
```
```kotlin
println(getWarmth(Color.ORANGE))		// warm
```

- 상수 값을 임포트하면 코드를 더 간단하게 만들 수 있다.

```kotlin
import study.colors.Color		// 다른 패키지에서 정의한 Color 클래스를 임포트
import study.colors.Color.*	// 짧은 이름으로 사용하기 위해 enum 상수를 모두 임포트
		
fun getWarmth(color: Color) = 
	when(color) {
		RED, ORANGE, YELLOW -> "warm"
		GREEN -> "neutral"
		BLUE, VIOLET -> "cold"
	}
```

### when과 임의의 객체를 함께 사용

- 분기 조건에 임의의 객체를 사용할 수 있다.

```kotlin
fun mix(c1: Color, c2: Color) =
	when (setOf(c1, c2)) {				// when 식의 인자로 아무 객체나 사용할 수 있다. 인자로 받은 객체가 각 분기 조건에 있는 객체와 같은지 테스트한다.
		setOf(RED, YELLOW) -> ORANGE
		setOf(YELLOW, BLUE) -> GREEN
		setOf(BLUE, VILOET) -> INDIGO
		else -> throw Exception("Dirty color")
	}
```

- 코틀린 표준 라이브러리에는 인자로 전달받은 여러 객체를 그 객체들을 포함하는 집합인 Set 객체로 만드는 setOf라는 함수가 있다.
