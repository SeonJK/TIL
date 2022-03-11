# GridLayout vs GridView
 두가지는 Grid라는 공통점으로 2차원 배열적으로 아이템들을 배열하는 공통점이 있다. 하지만 layout과 view라는 차이점이 있는데 자세한 두 가지의 차이점을 알아보자.
## > GridLayout
* 아이템들을 index로 참조할 수 있다.
* 화면 스크롤이 불가능하다.
* Adapter 사용X => 개발자가 직접 입력하고 인터페이스를 구성해야 한다.

## > GridView
* 화면 스크롤이 가능하다.(자동 스크롤)
* Adapter 사용O => ListAdapter를 이용하여 아이템을 가져온다.

# Stack
계산기 어플을 구현하면서 계산 로직을 Stack을 이용해 작업하려고 한다. Kotlin에서 Stack을 구현하는 방법을 알아보자.
* Stack으로 구현하려는 이유<br/>
자료구조나 컴퓨터구조 책에서 계산의 예시로 많이 드는 것이 Stack이다. 예시가 나온다는 건 그만큼 계산에 있어서 효율적이라는 이야기가 아닐까?

## > Kotlin에는 Stack 라이브러리가 없다!!
JAVA에서는 Stack 라이브러리가 있지만 Kotlin에서는 Stack 라이브러리가 없다. 때문에 직접 구현하거나 JAVA 라이브러리를 가져와 사용해야만 한다.<br/>

### **그럼 어떤 자료구조가 더 성능이 좋을까?**
이에 대해 김초희님이 [블로그](https://choheeis.github.io/newblog//articles/2020-10/kotlinStack)에 정리해 놓으셨다.

블로그에서는 3가지 방법을 비교하고 있다.
1. MutableList
2. ArrayList
3. JAVA 라이브러리<br/>
<hr/>

## > MutableList
MutableList는 전에 배웠던 것처럼 list 아이템을 변경할 수 있는 list이다.
```kotlin
fun main() {
    var mutableList = mutableListOf<Int>()

    // push = add()
    mutableList.add(1)
    mutableList.add(2)

    // pop = removeAt(가장 마지막 인덱스)
    mutableList.removeAt(mutableList.size-1)

    // top = 배열의 가장 마지막 원소
    println(mutableList[mutableList.size-1])

    // isEmpty or isNotEmpty
    println(mutableList.isEmpty())
    println(mutableList.isNotEmpty())
}
```

## > ArrayList
ArrayList는 MutableList 인터페이스를 상속받는 구현체이다.

```kotlin
fun main() {
    var arrayList = arrayListOf<Int>()

    // push = add()
    arraList.add(1)
    arraList.add(2)

    // pop = removeAt(가장 마지막 인덱스)
    arrayList.removeAt(arrayList.size-1)

    // top = 배열의 가장 마지막 원소
    println(arrayList[arrayList.size-1])

    // isEmpty or isNotEmpty
    println(arrayList.isEmpty())
    println(arrayList.isNotEmpty())
}
```

## > JAVA 라이브러리
라이브러리를 import 하여 push() / pop() 메소드를 그대로 사용하면 된다.
```kotlin
import java.util.*

fun main() {
    val javaStack = Stack<Int>()

    // push
    javaStack.push(1)
    javaStack.push(2)

    // pop
    javaStack.pop()

    // top = peek()
    println(javaStack.peek())
}
```

## 3가지 방법의 성능비교
3가지 방법의 push와 pop 시간을 측정하여 비교해봤다.
```kotlin
fun main() {
    // 1. 자바 Stack 사용시 push, pop 시간 측정
    var javaStack = Stack<Int>()
    println("Java Stack Push Time : " + measureNanoTime {
        for(i in 0..100000){
            javaStack.push(i)
        }
    })
    println("Java Stack Pop Time : " + measureNanoTime {
        for(i in 0..100000){
            javaStack.pop()
        }
    })

    // 2. 코틀린 MutableList 사용시 push, pop 시간 측정
    var mutableListStack = mutableListOf<Int>()
    println("Kotlin MutableList Push Time : " + measureNanoTime {
        for(i in 0..100000){
            mutableListStack.add(i)
        }
    })
    println("Kotlin MutableList Pop Time : " + measureNanoTime {
        for(i in 0..100000){
            mutableListStack.removeAt(mutableListStack.size-1)
        }
    })

    // 3. 코틀린 ArrayList 사용시 push, pop 시간 측정
    var arrayListStack = arrayListOf<Int>()
    println("Kotlin ArrayList Push Time : " + measureNanoTime {
        for(i in 0..100000){
            arrayListStack.add(i)
        }
    })
    println("Kotlin ArrayList Pop Time : " + measureNanoTime {
        for(i in 0..100000){
            arrayListStack.removeAt(arrayListStack.size-1)
        }
    })
}
```

결과는 다음과 같이 나왔다.
> Java Stack Push Time : 12273466<br/>
Java Stack Pop Time : 11367012<br/>
Kotlin MutableList Push Time : 4129391<br/>
Kotlin MutableList Pop Time : 2212038<br/>
Kotlin ArrayList Push Time : 2286275<br/>
Kotlin ArrayList Pop Time : 630148<br/>

## > 결론 - 빠른 순서: ArrayList > MutableList > JavaStack
<br/>

# Snackbar vs Toast
안드로이드에서 사용자에게 메시지를 간단하게 던져줄 때 두 가지를 사용한다.<br/><br/>
예전에 안드로이드 개발을 했을 때는 Toast를 많이 사용하였다.
```kotlin
// Toast 메시지
Toast.makeText(applicationContext, "message", Toast.LENGTH_SHORT).show()

// Snackbar 메시지
Snackbar.make(binding.root, "message", Snackbar.LENGTH_SHORT).show()
```

사용방법은 거의 비슷한데 무슨 차이가 있는지 살펴보자.

## > Toast
* 첫번째 parameter로 Context를 받는다.
* 메시지를 보여줄 때 별도의 공간을 차지하지 않는다.
* 텍스트가 두 줄로 제한된다.
* Android 12(API 31) 이상부터 어플리케이션 아이콘이 함께 표시된다.
* 주로 백그라운드 메시지를 출력할 때 사용한다.

## > Snackbar
* 첫번째 parameter로 view를 받는다.
* 메시지를 보여줄 때 별도의 공간을 차지한다.<br/>
=> <code>LENGTH_INDEFINITE</code>을 통해 다른 Snackbar가 나오기 전까지 지속시킬 수 있다.
* 간단한 텍스트를 보여줄 때 좋다.
* 메시지와 함께 Action을 추가할 수 있다.
* Material Design에서 권장한다.
* 주로 해당 앱이 포그라운드에 있을 때 사용한다.

# 오늘 봤던 오류 모음
## > NotImplemetedError
이 에러는 TODO로만 놔두고 구현하지 않았을 경우 생기는 에러이다.<br/>
**해결법**<br/>
TODO를 없애거나 주석처리 한다.

## > Resources$NotFoundException: String resource ID #0x0
이 에러는 textView의 setText() 함수 인자로 String이 넘어가지 않아서 발생한다.<br/>
**해결법**<br/>
setText()의 parameter를 확인하여 문자열로 만들어준다.
