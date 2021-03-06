---
sort: 5
---

# Kotlin In Action - Chapter_5 람다로 프로그래밍

람다 식, 또는 람다는 기본적으로 다른 함수에 넘길 수 있는 작은 코드 조각이다.

## 5.1 람다 식과 멤버 참조

"이벤트가 발생하면 이 핸들러를 실행하자"나 "데이터 구조의 모든 원소에 이 연산을 적용하자"와 같은 생각을 위해 일련의 동작을 변수에 저장하거나 다른 함수에 넘겨야 하는 경우가 있다.

자바에서는 ananymous inner class를 사용했지만 상당히 번거롭다.

함수형 프로그래밍에서는 함수를 값처럼 다루는 방법을 선택함으로써 이 문제를 해결한다. 람다 식을 사용하면 함수를 선언할 필요가 없고 코드 블록을 직접 함수의 인자로 전달할 수 있다.

### 5.1.1 람다 소개 : 코드 블록을 함수 인자로 넘기기

람다를 메소드가 하나뿐인 object class 대신 사용할 수 있다. 4장에서 배웠듯 말이다

### 5.1.2 람다와 컬렉션

컬렉션을 다룰 때 수행하는 대부분의 작업은 몇 가지 일반적인 패턴에 속한다. 따라서 그런 패턴은 라이브러리 안에 있어야 하고, 람다가 없다면 컬렉션을 편리하게 처리할 수 있는 좋은 라이브러리를 제공하기 힘들다. 

그래서 자바 8 이전 자바에서 쓰기 편한 컬렉션 라이브러리가 적었고, 따로 필요한 컬렉션 기능을 직접 작성했지만, 코틀린에서는 그럴 필요가 없다. 

자바 컬렉션에 대해 수행하던 대부분의 작업은 람다나 멤버 참조를 인자로 취하는 라이브러리 함수를 통해 개선할 수 있다. 프로퍼티를 반환하더라도 멤버 참조 사용 가능하다.

### 5.1.3 람다 식의 문법

코틀린 람다 식은 항상 중괄호로 둘러싸여 있다. 인자 목록 주변에 괄호가 없다는 사실을 기억해야한다. 화살표가 인자 목록과 람다 본문을 구분해준다. 

원한다면 람다 식을 직접 호출해도 된다.

~~~kotlin
{ println(42) } () 

run { println(42) } 
~~~

하지만 굳이? 위 보다는 아래가 낫다.

실행 시점에서 코틀린 람다 호출에는 아무 부가 비용이 들지 않는다. 프로그램의 기본 구성 요소와 비슷한 성능을 낸다.

진화 과정을 보도록 하자!

~~~kotlin
people.maxBy({p:Person -> p.age})
~~~

코틀린에는 함수 호출 시 맨 뒤에 있는 인자가 람다 식이라면 그 람다를 괄호 밖으로 빼낼 수 있는 문법 관습이 있다. 괄호 뒤에 람다를 둘 수 있다.

~~~kotlin
people.maxBy(){p:Person -> p.age}
~~~

만약 유일한 인자이고 괄호 뒤에 람다를 썼다면 호출 시 괄호를 없애도 된다!
~~~kotlin
people.maxBy{p:Person -> p.age} 
~~~

이런식이다. 제법 깔끔해졌다.

로컬 변수처럼 컴파일러는 람다 파라미터의 타입도 추론할 수 있다. 따라서 파라미터 타입을 명시할 필요가 없다.

maxBy 함수는 파라미터 타입이 항상 컬렉션 원소 타입과 가다. 람다의 파라미터도 Person이라는 사실을 안다.

못하는 경우도 있지만 여기선 다루지 않는다(?) -> 아마 좀 깊은 내용인듯?

그냥 처음에 람다를 작성하고 컴파일러가 타입을 모르겠다고 하는 경우에만 타입을 명시한다.

~~~kotlin
people.maxBy{p -> p.age}
~~~

더 간단.

디폴트 파라미터 이름은 it이다. 

~~~kotlin
people.maxBy{it.age}
~~~

 최종본? 이다. 매우 간단해졌다!

it을 사용하는 관습은 코드를 아주 간단하게 만들어주지만, 람다 안에 람다가 중첩되는 경우나 파악하기 어려운 경우가 있어 남발하면 안된다.

람다를 변수에 저장할 때는 파라미터의 타입을 추론할 문맥이 존재하지 않는다. 따라서 파라미터 타입을 명시해야 한다. 

본문이 여러줄일 경우 맨 마지막에 있는 식이 람다 값의 결과 값이 된다.

### 5.1.4 현재 영역에 있는 변수에 접근

자바 메소드 안에서 final인 로컬 변수를 무명 내부 클래스에서 사용할 수 있다. 그것처럼 람다도 같은 일을 할 수 있다. 

함수 안에서 정의하면 함수의 파라미터 뿐만 아니라 로컬 변수까지 람다에서 사용 가능하다.

final 변수가 아닌 변수에도 접근할 수 있다는 점이 자바와 다르다.

외부 변수를 람다가 사용하면 람다가 capture한 변수라고 부른다. 

함수가 반환되면 그 capture된 변수의 생명주기도 끝나지만, 어떤 함수가 자신의 로컬 변수를 포획한 람다를 반환하거나 다른 변수에 저장한다면 생명 주기가 달라질 것이다. 

포획한 변수를 가지고 있는 람다가 변수에 저장되어 원래 함수가 끝나더라도 포획한 변수를 쓸 수 있다!

파이널 변수를 포획한 경우, 람다 코트를 변수와 함께 저장하고, 파이널이 아닌 경우에는 변수를 특별한 래퍼로 감싸 나중에 변경하거나 읽을 수 있게 하고, 래퍼에 대한 참조를 람다 코드와 함께 저장한다.

옮긴이 - 람다를 클로저라고 부르기도 한다. 람다, 무명함수, 함수 리터럴, 클로저를 서로 혼용하는 경우가 많단다.

람다를 이벤트 핸들러나 다른 비동기적으로 실행되는 코드로 활용하는 경우 함수 호출이 끝난 다음에 로컬 변수가 변경될 수도 있다는 점을 조심해야한다. - > 이 부분은 명확치 않다고 느껴진다. 다시 공부해보도록 하자.

~~~kotlin
abstract class Button {
    abstract fun onClick(callback: () -> Int)
}

class SoButton : Button() {
    override fun onClick(callback: () -> Int) {
        println(callback())
        println("!")
    }
}

fun tryToCountButtonClicks(button: Button): Int {
    var clicks = 0
    button.onClick {
        clicks += 1
        println(clicks + 2)
        clicks
    }
    return clicks
}
~~~

### 5.1.5 멤버 참조

::을 사용하는 식을 멤버 참조라고 부른다. member reference!

멤버를 호출하는 람다와 같은 타입이라고 생각하면 된다. rxjava에서 봤던 녀석이지.

최상위에 선언된 함수나 프로퍼티를 참조할 수도 있는데, 

~~~kotlin
fun salute()=println("bye")
run(::salute)
~~~

이런 식이다. run 라이브러리 함수에 넘기는 것이다.

람다를 정의하지 않고 직접 위임함수에 대한 참조를 제공하면 더 편리하다.

~~~kotlin
val nextAction = ::sendEmail 
~~~

이런 식으로.

생성자 참조를 할 수도 있다.

~~~kotlin
val createPerson = ::Person
~~~

이렇게! 생성자 이름이 바뀐다고 생각하면 편하려나? 인스턴스를 만드는 동작을 값으로 정의해버린다고 보면 된다.

확장 함수도 멤버 함수와 마찬가지로 참조할 수 있다.

#### 바운드 멤버 참조

코틀린 1.0에서는 멤버 참조를 할 때 클래스 인스턴스를 따로 넣어줘야 했지만

1.1부터는 자동으로 인스턴스를 생성해 인스턴스를 별도로 지정해 줄 필요가 없다!!

인스턴스를 따로 넣어줘야하는 귀찮음은... 잘 바뀐거 같다 확실히.

## 5.2 컬렉션 함수형 API

함수형 프로그래밍을 사용하면 컬렉션을 다룰 때 편히하다. 다 다른 언어에 보통 있는 내용들이다.

### 5.2.1 필수적인 함수 filter, map

filter 함수는 컬렉션을 이터레이션 하면서 람다에 각 원소를 넘겨 람다가 true를 반환하는 원소만 모은다.

~~~kotlin
val list = listOf(1,2,3,4)

println(list.filter { it % 2 == 0 }) 
//[2, 4]
~~~

이런 식이다.

원소를 제거하지만 변환할 수는 없다.

변환하려면 map을 써야한다. filter와 비슷하게 생각하면 된다.

### 5.2.2 all any count find 컬렉션에 술어 적용

all -> 다 만족하는가?

any -> 하나라도 만족하는가?

count -> 만족하는 갯수

find -> 가장 처음 만족하는 녀석 반환 , 안되면 null. firstOrNull과 같다.

가독성을 높이려면 all하고 any앞에 !는 없애는게 낫다. 드 모르강의 법칙 사용.

### 5.2.3 groupBy: 리스트를 여러 그룹으로 이뤄진 맵으로 변경

어떤 특성에 따라 나누고 싶을 때, 키 값에 따른 각 그룹이 값인 맵(파이썬의 딕셔너리)으로 바꿔준다.

각 그룹은 리스트이기 때문에 groupBy의 결과 타입은

Map<key값,List<컬렉션의 원소>>이 될 것이다.

그냥 나눈다는 거다.

### 5.2.4 flatMap, flatten 중첩된 컬렉션 안의 원소 처리

flatMap은 리스트의 리스트에 들어있던 걸 한 리스트로 만들어주는 녀석이다.

근데 변환을 안해도 된다? 하면 평평하게 펼칠 뿐인 flatten을 쓰는거지.

listOfList.flatten() 이런 식으로

## 5.3 지연 계산 (lazy) 컬렉션 연산

컬렉션 함수를 연쇄하면 매 단계마다 계산 중간 결과를 새로운 컬렉션에 임시로 담는데, 시퀀스sequence를 사용하면 중간 임시 컬렉션을 사용하지 않고도 연산을 연쇄할 수 있다.

원본 컬렉션을 시퀀스로 반환하고 결과 시퀀스를 다시 원래 컬렉션으로 변환하면 된다.

이때 시퀀스로 반환한 녀석들에게도 컬렉션과 똑같은 API가 제공된다.

큰 컬렉션에 대해서 연산을 연쇄시킬 때는 시퀀스를 사용하는 것을 규칙으로 삼아야 한다.

시퀀스가 더 낫다면 그냥 시퀀스를 쓰면 되지 않는가? 항상 그렇지는 않다. 차례로 이터레이션 한다면 시퀀스가 낫지만 인덱스를 사용해 접근하는 등의 다른 API를 쓴다면 시퀀스를 리스트로 반환해야 한다.

### 5.3.1 시퀀스 연산 실행 : 중간 연산과 최종 연산.

중간 연산은 다른 시퀀스를 반환하고, 최종 연산은 결과를 반환한다.

중간 연산은 항상 지연 계산된다. 하나 하나 먼저 전체를 수행해본다는 점이 다른 느낌이랄까.

map(1) filter(1) map(2) filter(4) ...

하나 하나 함에 있어 sequence에서는 filter를 map보다 먼저하는 게 좋다는 것을 알 수 있을 것이다.

### 5.3.2 시퀀스 만들기

.asSequence()를 호출에 시퀀스를 만들 수 있다 generateSequence 함수도 이용할 수 있는데, 이건 이전의 원소를 인자로 받아 다음 원소를 계산한다.

val num = generateSequence(0){t+1}

이것도 마찬가지로 막판에 계산되게 할 수 있는 것이다.

일반적인 용례 중 하나는 객체의 조상으로 이뤄진 시퀀스를 만드는 것이다.

예를 들어 상위 디렉터리를 뒤지면서 숨김 속성을 가진 디렉터리가 있는지 검사함으로써 파일이 감춰진 디렉터리 안에 있는지 알아본다.

## 5.4 자바 함수형 인터페이스 활용

우리가 다뤄야하는 api중 대다수는 코틀린이 아닌 자바로 작성된 api이다. 

SAM은 단일 추상 메소드이다. 예를 들어 버튼을 보자.

~~~java
button.setOnClickListener(

@Override

public void onClick(View v){

}
)
~~~

이게 자바 8이전의 버전이다. 무명 클래스 인스턴스를 넘긴 것이다.

여기서 원래 setOnClickListener의 인자는 onClickListener로 onClick이라는 메소드만 정의되어 있다.

코틀린 코드를 보면

~~~kotlin
button.setOnClickListener{view -> }
~~~

이렇게 된다. 이게 된다고? 왜?

~~~kotlin
public void onClick(View v){

}
~~~
여기서 View라는 파라미터가 있다. 이게 view로 되는 것이다.

왜 그러냐? onClickListener에 추상 메소드가 onClick하나만 있어서다. 그런 인터페이스를 함수형 인터페이스 혹은 SAM 인터페이스라고 한다.

Runnable, Callable도 마찬가지다. 자바 API에는 그런 애들이 많다. 그래서 코틀린에서 무명 클래스 인스턴스를 정의하고 활용할 필요가 없어서 코틀린 다운 코드로 남아있을 수 있다.

### 5.4.1 자바 메소드에 람다를 인자로 전달

~~~java
void  postponeComputation (int delay, Runnable computation);
~~~

~~~kotlin
postponComputation(1000) {println(42)}
~~~

이제 이게 이해가 될 것이다. Runnable의 run이 그런 추상메소드인 것이다!

무명 객체와는 또 차이가 있는데
~~~kotlin
postponComputation (1000, object:Runnable{
    override fun run(){
        println(42)
    }
})
~~~kotlin

이거하고는 차이가 있다. 이 경우에는 매번 새로운 객체가 생성된다. 

람다는 프로그램 전체에서 Runnable의 인스턴스가 단 하나만 만들어진다.

object를 쓰면서 그런 경우는 어떤 경우인가

~~~kotlin
val runnable =Runnable {println(42)}

fun handleComputation(){

postponeComputation(1000,runnable)

}
~~~kotlin

이렇게 runnable이 전역 변수처럼 되는 경우인 것이다. 근데! 만약에 람다가 주변 영역의 변수를 포획한다면 매번 새로운 인스턴스를 생성해야한다.

~~~kotlin
fun handleComputation(id){

postponeComputation(1000){println(id)}

}
~~~

이렇게 말이다.

### 5.4.2 SAM 생성자 : 람다를 함수형 인터페이스로 명시적으로 변경

이름이 함수형 인터페이스와 같다.

그 함수형 인터페이스의 유일한 추상 메소드의 본문에 사용할 람다만을 인자로 받아서 함수형 인터페이스를 구현하는 클래스의 인스턴스를 반환한다.

가끔 모호한 경우가 있을 때도 이걸 쓴다.

람다는 자기 자신을 가르키는 this가 없다. 람다는 코드 블록일 뿐이고, 객채가 아니므로 객체처럼 람다를 참조할 수는 없다. 

람다 안에서 this는 그 람다를 둘러싼 클래스의 인스턴스를 가르킨다. 

이벤트 리스너가 이벤트를 처리하다가 자기 자신의 리스너 등록 해제를 해야한다면 람다를 사용할 수 없다. 

그런 경우 람다 대신 무명 객체를 사용해 리스너를 구현해야한다. ​무명객체는 this가 인스턴스 자신을 가르키므로.

## 5.5 수신 객체 지정 람다 : with와 apply

### 5.5.1 with

첫번째 인자로 받은 객체를 두번째 인자로 받은 람다의 수신 객체로 만든다

어려울게 없다. this로 참고한다. with가 반환하는 값은 람다 코드를 실행한 결과이기에 그 결과는 람다 식의 본문 마지막 식의 값이다.

### 5.5.2 apply 

with와의 차이는 수신 객체를 반환한다는 점이다.

### 유용한 수신 객체 지정

[링크](https://medium.com/androiddevelopers/kotlin-standard-functions-cheat-sheet-27f032dd4326)