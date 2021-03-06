---
sort: 4
---

# Kotlin In Action - Chapter_4 클래스, 객체, 인터페이스

## 4.1 클래스 계층 정의

### 4.1.1 코틀린 인터페이스

자바랑 비슷하게 시작. 인터페이스를 구현하는 모든 비추상 클래스는 메소드에 대한 구현을 제공해야한다.

자바에서는 extends와 implements 키워드를 사용하지만, 코틀린에서는 가볍게 클래스 명 뒤에 : 만 붙인다.

클래스와 인터페이스 처리를 모두 동일하게 처리한다. 자바와 마찬가지로 인터페이스는 여러개, 클래스는 하나만 상속할 수 있다.

자바에서는 @Override를 써도 되고 안써도 됐지만, 코틀린은 override라고 꼭 명시해야한다.

실수로 오버라이드 하는 경우를 방지해 준다.

디폴트 구현이 있다. 자바에서는 자바8에 추가되었는데, default를 써야하지만, 코틀린에선 상관없다. 이건 무조건 넣어준다는 의미이다. override해도 상관없다.

근데 이건 그냥 메소드 본문을 시그니처 뒤에 추가하면 끝이다! 되게 간단하다.

어? 그럼 만약에 두 인터페이스에서 같은 시그니처를 가진 메소드를 구현한다면?

오버라이드를 직접 하지 않으면 컴파일러 오류가 생긴다. 오버라이드를 직접 해서 디폴트를 쓰고 싶다?

~~~kotlin
override fun showoff() {
    super<Clickable>.showoff()
    super<Focusable>.showoff()
}
~~~

이런 식으로 super와 꺾쇠 괄호를 활용한다.

어떤 상위 타입의 멤버 메소드를 호출할 지 지정할 수 있는 것이다!

자바에서 코틀린의 메소드가 있는 인터페이스를 구현할 때는 좀 골치가 아파진다.

코틀린은 자바 6과 호환되게 설계되었다.

그래서 자바8인 인터페이스 디폴트 메소드를 지원하지 않는다. 코틀린은 그럼 어떻게 구현되어져 있는가?

일반 인터페이스와 디폴트 메소드 구현이 정적 메소드로 들어있는 클래스를 조합해 구현되어있는 것이다.

이 인터페이스에는 메소드 선언들이 다 들어가있다. 그러면 자바에서는 이걸 다 오버라이드해줘야한다.

즉, 모든 메소드에 대한 본문을 작성해줘야 하는 것이다.

자바에서는 그래서 사실상 그 디폴트 메소드를 쓸 수 없다고 봐야한다.

​
### 4.1.2 open, final, abstract 변경자 : 기본적으로 final

자바에서는 final로 명시적으로 상속을 금지하지 않는 모든 클래스를 다른 클래스가 상속할 수 있다. 하지만 문제가 생기는 경우가 많다. 

취약한 기반 클래스. fragile base class라는 문제는, 하위 클래스가 기반 클래스에 대해 가졌던 가정이 기반 클래스를 변경함으로써 깨져버린 경우에 생긴다. 

어떤 클래스가 자신을 상속하는 방법에 대해 정확한 규칙(예를 들면 어떤 메소드를 어떻게 오버라이드를 해야 하는지 등)을 제공하지 않는다면 

그 클래스의 클라이언트(아마 하위 클래스를 말하는 듯 하다.)는 기반 클래스를 작성한 사람의 의도와 다른 방식으로 메소드를 오버라이드 할 위험이 있다.

모든 하위 클래스를 분석하는 것은 불가능하므로, 기반 클래스를 변경하는 경우 하위 클래스의 동작이 예기치 않게 바뀔 수도 있다는 면에서 기반 클래스는 취약하다. 

상속을 위한 설계와 문서를 갖추거나, 그럴 수 없다면 상속을 금지하라. - Effective Java

그래서, 자바에서는 final을 하지 않으면 기본적으로 상속에 열려있지만, 코틀린은 기본적으로 final이다.

생각해보면 상속이 필요없는 메소드들에 대해 open 되어있는 것 보다는 기본적으로 final이 확실히 맞는것 같다.

코틀린은 그래서 open을 붙여줘야 오버라이드 가능하다.

기본적으로 final이고, open을 붙여줘야하는데, open을 오버라이드 한 녀석은 open이다.

그래서 오버라이드 한 애를 또 오버라이드 시키고 싶지 않다면 final을 붙여줘야 한다.

### 열린 클래스와 스마트 캐스트.

final로 기본 상태를 함으로써 얻는 큰 이득은 다양한 경우에 스마트 캐스트가 가능하다는 점이다.

스마트 캐스트를 다시 보면, is로 검사하고 나면 굳이 변수를 원하는 타입으로 변화시키지 않아도 마치 처음부터 그 변수가 원하는 타입으로 선언된 것처럼 사용할 수 있다는 점이다. 실제로 컴파일러가 해주는 작업이다.

이 때 제한조건은 val이고, 커스텀 접근자를 안 쓴 경우. 이 요구사항이 프로퍼티가 final이어야한다는 의미이기도 하다. 

final이 아니라면 그 프로퍼티를 다른 클래스가 상속하고, 커스텀 접근자를 정의하면서 스마트 캐스트의 요구 사항을 바꿀 수도 있으니까.

​___

자바처럼 코틀린도 클래스에 abstract 선언할 수 있다. abstract로 선언한 추상 클래스는 인스턴스화 할 수 없다. 자바랑 같다.

추상 멤버가 있기 마련인데, 이건 하위 클래스에서 꼭 구현을 하게 한다. 그래서 open도 명시하지 않는다. 당연한거니까.

쉽게 표로 보자.

Header 1 | Header 2
--------- | ---------
final | 오버라이드 불가능 - 기본
open | 오버라이드 가능
abstract | 추상 클래스의 멤버에만 이걸 붙일 수 있다. 추상 클래스에는 구현이 있으면 안된다.
override | 오버라이드 하는 멤버는 기본적으로 열려있다. 하위 클래스에서 오버라이드를 금지하려면 final을 명시한다.

### 4.1.3 가시성 변경자 : 기본적으로 공개

모듈은 한꺼번에 컴파일 되는 코틀린 파일들이다.

변경자 | 클래스 멤버 | 최상위 선언 (클래스, 함수, 프로퍼티 선언)
--------- | --------- | ---------
public | 모든 곳에서 볼 수 있다. |모든 곳에서 볼 수 있다.
internal | 같은 모듈 안에서만 볼 수 있다. | 같은 모듈 안에서만 볼 수 있다.
protected | 하위 클래스 안에서만 볼 수 있다. | 최상위 선언 적용 불가능
private | 같은 클래스 안에서만 볼 수 있다. | 같은 파일 안에서만 볼 수 있다.

참고로 [모듈 vs 패키지](https://stackoverflow.com/questions/57531779/what-is-the-difference-between-package-and-module-in-kotlin)

자바에서는 패키지 전용이 기본 가시성이었지만 코틀린은 네임스페이스를 관리하기 위한 용도로만 사용한다.

모듈은 한 번에 컴파일되는 코틀린 파일들을 의미한다.

어떤 클래스가 상속하려고 하면 그 상속하려는 클래스(기반 타입 목록)들은

제네릭 클래스에 들어가는 파라미터 애들의 타입 가시성은

그 클래스 자신의 가시성과 같거나 더 높아야하고, 

메소드에서는 메소드의 시그니처에 사용된 모든 타입의 가시성이 그 메소드의 가시성과 같거나 더 높아야한다.

2였나 3챕에서 말했었나, 클래스를 확장한 함수는 그 클래스의 private나 protected 멤버에 접근할 수 없다.  


~~~kotlin
internal open class TalkativeButton {
    fun oh() = println("public!")
    private fun yell() = println("private!")
    internal fun wow() = println("Internal!")
    protected fun whisper() = println("protected!")
}

internal fun TalkativeButton.say() { // fun만 쓰면 public이라 internal을 참조할 수 없다.
    oh()
    //yell() 컴파일 에러
    wow()
    //whisper() 컴파일 에러
}
~~~

### 코틀린의 가시성 변경자와 자바

자바에서 코틀린일 때는 접근 못하던 곳으로 접근 가능한 경우가 있다.

internal이나 protected가 그런 경우이다.

internal은 public이 되는데, 멤버의 이름을 '보기 나쁘게' 바꿈으로써 모듈 밖애서 사용하는걸 막는다.

또 다른 이유로는 우연히 밖에서 상속한 녀석이 internal 메서드를 오버라이드를 막기 위함이다.

코틀린에서 접근 못해도 자바에서 접근할 수 있는 경우가 생기기는 한다!

protected는 자바에서와 코틀린에서 다르니까 어쩔 수 없는 부분으로 보는 것 같다.

private 와 internal은 자바에서 어떻게 해석되는가?

private는 패키지 전용 클래스로 컴파일한다.



### 4.1.4 내부 클래스와 중첩 클래스 : 기본적으로 중첩 클래스

자바의 내부 & 중첩 클래스 한 번 읽고 오자.

[자바 4대 중첩 클래스](https://gyrfalcon.tistory.com/entry/JAVAJ-Nested-Class)

쉽게 생각해서 익명 클래스 말고는 변수라고 생각하면 편하다. 

자바에서는 다른 클래스 안에 정의한 클래스는 자동으로 내부 클래스가 된다.

중첩 클래스는 static을 명시해줘야하는데, 코틀린은 이게 디폴트다..! 

inner class가 자바에서의 그냥 class, 즉 내부 클래스이다. 

정리해보자. 자바에서는 원래 class로 하면바깥 클래스에 대한 참조를 묵시적으로 포함한다. 

근데 코틀린에서는 참조하지 않는다. inner class해줘야한다.

inner class에서 바깥쪽 클래스는 this@Outer로 접근한다.

### 4.1.5 sealed 클래스. 클래스 계층 정의시 계층 확장 제한

인터페이스로 when을 처리할 때 한계가 있다. 그래서 sealed를 써볼 수 있다.

sealed 클래스의 하위 클래스를 정의할 때에는 반드시 상위 클래스 안에 중첩시켜야한다.

when으로 sealed 클래스를 검사하면 모든 하위 클래스를 검사하므로 else 분기가 필요없다.

sealed 인터페이스는 없는데, 그 인터페이스를 자바 쪽에서 구현 못하게 막을 수 있는 수단이 코틀린 컴파일러에게 없기 때문이다. 

## 4.2 뻔하지 않은 생성자와 프로퍼티를 갖는 클래스 선언

### 4.2.1 주 생성자와 초기화 불록

주 생성자와 부 생성자가 있다. 주는 class 이름 옆에 바로 적는 녀석.

~~~kotlin
class User (val nickname : String) {    
}
~~~

val을 붙이는건 프로퍼티로 가지겠다는 의미이다. 물론 디폴트 값도 정의할 수 있다.

constructor와 init이라는 키워드가 있다.

construct는 생성자. 명시적으로 하면 원래 주 생성자에도 있는데  어노테이션 등이 없다면 안해도 된다.

~~~kotlin
class User constructor (val nickname : String) {
}
~~~

요래.

private constructor도 있는데, 어떤 클래스를 외부에서 인스턴스화 못하게 막고 싶다면 그럴 수 있다.

그럼 어떻게 만들지..? 만들 필요가 없는 경우에 사용된다. - companion object가 대표적일 것이다.

생성자 없으면 디폴트 생성자 만들어준다. 이거 때문에 하위 클래스는 무조건 생성자를 호출해야 한다. 그래서 괄호를 친다. 

인터페이스는 생성자가 없어서 안한다. 그래서 인터페이스와 클래스 상속 구분가능하다! 와!

init은 초기화 블록. 주 생성자 할때 실행된다. 주 생성자는 딴데 쓸데가 없으니까.
​
### 4.2.2 부 생성자 : 상위 클래스를 다른 방식으로 초기화

디폴트 파라미터를 지정할 수 있기 때문에 부 생성자 쓸 필요는 거의 없다. 

~~~kotlin
constructor(ctx: String, mystyle: String) : this(ctx)

constructor(ctx: String) : super(ctx)
~~~

super로 상위 클래스 생성자를 호출할 수 있고 this로 클래스 자신의 다른 생성자를 호출할 수 있다. 

클래스에 주 생성자가 없다면 모든 부생성자는 반드시 상위 클래스를 초기화하거나 다른 생성자에서 생성해야한다.

파라미터 목록이 다른 생성 방법이 여럿 존재하는 경우에는 부 생성자를 여럿 둘 수 밖에 없다.

### 4.2.3 인터페이스에 선언된 프로퍼티 구현.

추상 프로퍼티 선언을 넣을 수 있는데, 이건 하위 클래스가 구현해야한다. 물론 이때도 override

이 구현 방법이 방식이 여럿 있는데, 주 생성자에서 부터 오바리이딩을 하던지, 커스텀 게터로 하던지

인터페이스에서 게터 세터 있는 프로퍼티 쓸 수 있는데, 이건 당연히 backing field 못쓴다.

인터페이스는 상태를 저장할 수 없으니까. 만약 게터가 따로 있다면 오버리이드 하지 않고 상속할 수도 있다. 이때도 당연히 백킹 필드는 없다는 점.

### 4.2.4 게터와 세터에서 뒷받침하는 필드에 접근

필드에 접근할 때 동시에 로직을 실행하려면? set에 넣고 field = value를 넣으면 될 것이다.

field를 안넣으면 backing field도 존재하지 않는다!!

### 4.2.5 접근자의 가시성 변경

접근자 get set앞에 가시성 변경자를 추가해서 가시성을 변경할 수 있다.

## 4.3 컴파일러가 생성한 메소드 : 데이터 클래스와 클래스 위임

### 4.3.1 모든 클래스가 정의해야 하는 메소드

자바와 마찬가지로 코틀린 클래스도 toString, equals hashCode 등을 오버라이드할 수 있다.

자바에서 == 는 원시 타입의 경우 두 피연산자의 값이 같은지 비교하는 거고, 참조 타입에서는 주소를 비교한다.

객체의 동등성은 equals를 이용해 비교해야한다. eqauls대신 ==를 호출하면 안된다.

코틀린에서는 == 가 두 객체를 비교하는 기본적인 방법이다. 내부적으로 이게 equals를 호출한다. 그래서 클래스가 equals를 오버라이드 하면 == 를 통해 인스턴스를 비교할 수 있다. ===는 자바에서의 ==와 같다!!

is는 자바의 instanceof와 같다.

자바에서는 equals를 오버라이드 할 때 반드시 hashCode도 함께 오버라이드 해야한다.

JVM언어에서는 "equals()가 true를 반환하는 두 객체는 반드시 같은 hashCode()를 반환해야 한다"라는 제약이 있다.

이 모든 것들을 하긴 힘들다 그래서 데이터 클래스를 사용해본다. 

### 4.3.2 데이터 클래스 : 모든 클래스가 정의해야 하는 메소드 자동 생성

data class로 나타내는 클래스를 데이터 클래스라고 부른다.

자동으로 위 오버라이드 해야하는 메소드를 생성해준다. 주 생성자에 나열된 모든 프로퍼티를 고려해서 만들어진다. 주 생성자 밖에 정의된 프로퍼티는 고려의 대상이 아니다!

데이터 클래스를 불변 객체로 더 쉽게 쓸 수 있도록 copy() 메소드를 알아두자. 

데이터 클래스의 프로퍼티는 var을 써도 되지만 val로 다 만들어서 불변 클래스로 만들라고 권장한다. 객체를 메모리상에서 직접 바꾸는 대신 복사본을 만드는 편이 낫다는 점에서 만들어진 것이다.

### 4.3.3 클래스 위임 : by 키워드 사용

대규모 객체지향 시스템을 설계할 때 시스템을 취약하게 만드는 문제는 보통 구현 상속에 의해 발생한다.

이런 문제때문에 코틀린은 기본적으로 클래스를 final로 취급한다. open을 보고 좀 더 조심스러워 질 수 있는 것이다.

하지만 종종 상속을 허용하지 않는 클래스에 대해 새로운 동작을 추가해야 할 떄가 있으니, 그때 데코레이터 패턴을 사용한다. 

그 클래스를 대신할 새로운 데코레이터 클래스를 만들되, 기존 클래스와 같은 인터페이스를 데코레이터가 제공하게 만들고, 기존 클래스를 데코레이터 내부에 필드로 유지하는 것이다. 이런 준비 과정은 준비 코드가 상당히 많이 필요하다.

by로 위임한다!

~~~kotlin
interface tree

class bao : tree {
    fun grow() {
        println("!")
    }
}

class baobab(val innerTree: tree = bao()) : tree by innerTree {
}
~~~

이렇게 코틀린에서는 위임이 쉽다. 물론 오버라이드 해서 위임을 안시키는 메소드 만들수도 있다!

## 4.4 object 키워드 : 클래스 선언과 인스턴스 생성

object declaration은 싱글턴은 정의하는 방법 중 하나다.

companion object는 인스턴스 메소드는 아니지만 어떤 클래스와 관련 있는 메소드와 팩토리 메소드를 담을 때 쓰인다. 

companion object에 접근할 때는 companion object가 포함된 클래스의 이름을 사용할 수 있다.

객체 식은 자바의 anonymous inner class 대신 쓰인다. 

### 4.4.1 객체 선언, 싱글턴 쉽게 만들기

객체지향에서 인스턴스가 하나만 필요한 클래스가 유용한 클래스가 많다. 이때 자바는 생성자를 private로 지정하고 정적인 필드에 그 클래스의 유일한 객체를 저장하는 싱글턴 패턴을 씀으로써 구현한다. 

근데 코틀린은 object로 싱글턴을 언어에서 기본 지원한다.

object Payroll{}

이렇게.

생성자 정의도 필요 없다. 왜? 객체 선언문이 있는 위치에서 호출 없이 즉시 만들어진다. 변수와 마찬가지로 객체 선언에 사용된 이름 뒤에 마침표를 붙이면 메소드나 프로퍼티에 접근 가능하다.

클래스 안에서 object 만들 수도 있는데, 이 때도 한개만 생성된다. 인스턴스마다 생기는 게 아니다.

자바에서는 유일한 인스턴스에 대한 정적인 필드가 있는 자바 클래스로 컴파일된다.

참고로 객체도 클래스나 인터페이스를 상속할 수 있다. ​

#### 싱글턴과 의존관계 주입

대규모 소프트웨어 시스템에서는 object가 항상 적합하지는 않다. 객체 생성을 제어할 방법이 없고, 생성자 파라미터를 지정할 수 없어서다. 

생성을 제어 못하고 파라미터를 지정 못하니까 단위 테스트를 하거나 소프트웨어 시스템의 설정이 달라질 때 객체를 대체하거나 의존관계를 바꿀 수 없다. 

그래서 의존관계 주입 프레임워크와 코틀린 클래스를 함께 사용해야 한다.

### 4.4.2 동반 객체, 팩토리 메소드와 정적 멤버가 들어갈 장소

companion object이다. 코틀린에는 정적인 애가 없다. 왠만하면 최상위 함수로 해결가능하니까.

근데 private로 표시된 클래스 비공개 멤버에 접근을 못하니까 따로 이걸 해결하려면 클래스 안에 만들어야겠지

그래서 만들어진 companion object이다. 클래스 안에 있던 object중 하나에 companion 이라는 표시를 붙이면 된다. 자바의 정적 메소드 호출이나 정적 필드 사용 구문과 같아진다.

바깥쪽 클래스의 private 생성자도 호출 할 수 있기 때문에 팩토리 패턴을 구현하기 가장 적합한 위치다.

주 생성자를 비공개로 만든 뒤 클래스 이름을 호출하고 companion object안의 메소드에서 생성하게 만드는 것이다.

클래스를 확장해야만 하는 경우에는 동반 객체 멤버를 하위 클래스에서 오버라이드 할 수 없다. 그래서 사실은 그런 경우엔 여러 생성자를 사용하는 편이 낫다.

### 4.4.3 동반 객체를 일반 객체처럼 사용 

companion object에 이름을 붙여 쓸수도 있고 그냥 클래스. 으로 사용해도 된다. companion object도 일반 object처럼 인터페이스, 클래스 상속 가능하다. 

인터페이스를 상속했으면 그 객체 자체의 이름으로 상속한 걸로 써먹을 수 있다. 이 부분은 직렬화가 익숙치 않아서인지 쉽지 않다.

자바에서 사용할 때는 10장 어노테이션을 참고하자. @JvmStatic 등이 있다. 대략적으로는 Companion이라는 정적 필드가 되는 느낌이 된다.

companion object에 확장 함수를 만들수도 있다. 이때도 fun 클래스이름.Companion.함수이름을 사용한다.

대표적으로 비즈니스 로직 모듈에서는 그냥 빈 companion object 만들고, 클라이언트 서버 통신 모듈에서 확장 함수 선언으로 직렬화 함수를 선언 한다는 느낌.

이때 없는 companion object에서 확장 함수를 만들 순 없으니 빈 companion object라도 만들어야 한다는 점에 주의.

### 4.4.4 객체 식: 무명 내부 클래스를 다른 방식으로 작성

object 키워드를 싱글턴과 같은 객체를 정의하거 그 객체에 이름 붙일 때만 사용하지는 않는다. 

anonymous object!를 쓸때도 쓴다. 이건 알다시피 자주 쓴다! 

이름을 붙여야 한다면 변수에 무명 객체를 대입하면 된다.

당연히 무명 객체는 싱글턴이 아니다.

한 인터페이스만 구현하거나 한 클래스만 확장할 수 있는 자바의 anonymous object와는 달리 코틀린은 가능하다. 여러 인터페이스 구현이나 클래스 확장하면서 인터페이스 구현같은거!

자바에서는 객체 식 안의 코드는 밖에 있는 변수는 final만 쓸 수 있는데, 코틀린은 상관없다! 다 쓸 수 있다.

이 객체 식은 여러 메소드를 오버라이드 해야 하는 경우에 쓰는게 좋다! 메소드 하나뿐이면 SAM 변환 지원을 쓰는게 낫다.