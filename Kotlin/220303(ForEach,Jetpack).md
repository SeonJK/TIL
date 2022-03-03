# For vs ForEach
코틀린에서 foreach문은 for문으로 대체할 수 있다. 그럼 foreach문이 존재하는 이유는 무엇일까? 그 이유를 알아보자.
```kotlin
fun main(){
    for( i in 1..10)
        println(i)
    
    (1..10).forEach{ i ->
        println(i)
    }
}
```

## > 일반적인 반복문
일반적으로 단순 계산작업을 하는 반복문에서는 for문의 시간이 훨씬 빠르다.<br/>
왜냐하면 foreach문은 람다 형식으로 함수를 계속 호출하는 형태이기 때문이다.

## > Collection에서의 반복문
Collection 클래스를 상속받는 List, Set, Map에서 반복문을 수행할 때 foreach문은 훨씬 빠른 시간으로 수행할 수 있다.
### why?
Collection 클래스를 상속받는 클래스들은 inline을 제공하기 때문<br/>
* ※ inline이란? <br/> 람다식을 사용했을 때 무의미한 객체 생성을 막는 코드
```kotlin
fun main(){
    val list = (1..10000).toList()

    println("ForLoop Time: "+ measureNanoTime {
        for(i in list) { loop(i) }
    })  // ForLoop Time: 1763600

    println("ForEach Time: "+ measureNanoTime {
        list.forEach{ i -> loop(i) }
    })  // forEach Time: 546600
}
```
## > asSequence()
asSequence() 함수를 사용하면 List 호출 시 최적화를 진행할 수 있다.<br/>
보통 체인 형식으로 Collection이 호출될 경우(list -> filter -> map) List는 값들을 저장하기 위해 temp Collection을 따로 생성 후 값을 저장하게 된다.<br/>
asSequence()를 통해 lazy Collection을 활용하여 temp Collection의 생성하지 않고 즉시 계산을 수행한다.
```kotlin
list.asSequence()
```

## > break, continue 기능
Kotlin에서는 forEach문을 사용해 JAVA의 break와 continue 기능을 구현할 수 있다.
```kotlin
fun main(){
    println(measureNanoTime {
        // continue 기능
        // @forEach는 list.forEach로 돌아가도록 한다.
        list.forEach{ i -> if(i>5000) return @forEach
        else loop(i) }
    })

    println(measureNanoTime {
        // break 기능
        // forEach문에서 @loop를 만나면 빠져나온다.
        run loop@{
            list.forEach { i -> if(i>5000) return @loop
            else loop(i)
            }
        }
    })
}
```

# 심리테스트 앱 예제 만들기
Code with Joyce님의 튜토리얼 마지막 강의에서 심리테스트 앱을 만드는 데 이를 보며 배운 것들을 남겨보고자 한다.
## > Jetpack
안드로이드를 개발할 때 쓰는 라이브러리로, 누구든지 고급진 어플을 개발할 수 있도록 정리해놓은 라이브러리이다.<br/> 예전엔 Support library란 이름으로 개발을 도와주는 라이브러리가 존재했으나 많은 문제점들이 있었다. 이를 개선하고 이름을 바꿔 나온게 Jetpack이라고 한다. Jetpack은 AndroidX 사용을 권장한다.