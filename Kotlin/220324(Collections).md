# Iterable vs Sequences
컬렉션을 공부하니까 Iterable, Sequence 등이 나와서 검색을 해보게 되었다. 처음 보는 개념들이어서 이해하는 데 조금 시간이 걸렸지만 이해한 것들을 정리해보고자 한다.

Kotlin에서는 Iterator, Collection, List, Map, Set을 모두 Collections라고 부른다. Sequence는 Iterable한 자료구조이다. 또한 Lazy evaluation으로 동작한다. (Iterable은 Eager evaluation으로 동작한다.)   
<br/>
Eager, Lazy evaluation을 설명하려면 Java Stream과 비교하며 설명하게 된다. Java Stream은 Lazy evaluation으로 동작한다.

* Kotlin Iterable: Eager evaluation
* Koltin Sequences: Lazy evaluation
* Java Stream: Lazy evaluation

## > Lazy evaluation vs Eager evaluation
Lazy evaluation은 지금 하지 않아도 되는 연산은 최대한 뒤로 미루고, 필요할 때 연산을 수행한다. Lazy evaluation은 효율을 중시(?)하는 연산이기 때문에 filter()와 map() 연산을 연속적으로 실행하여 filter()에 해당하는 요소에 대해 map() 연산을 바로 실행한다. take() 연산의 기준이 충족되면 뒤의 요소에 대한 연산은 수행하지 않는다.  
<br/>
Eager evaluation은 수행할 연산을 바로 수행한다. Eager evaluation은 우직하게 모든 연산을 다하기 때문에 filter() 연산을 모든 요소에 대해 수행하고 take() 연산의 기준이 충족되더라도 조건에 맞는 모든 요소에 대해 map()연산을 차례대로 수행한다.

## > 예제
* Iterable

```kotlin
val words = "The quick brown fox jumps over the lazy dog".split(" ")
val lengthsList = words.filter { println("filter: $it"); it.length > 3 }
    .map { println("length: ${it.length}"); it.length }
    .take(4)

println("Lengths of first 4 words longer than 3 chars:")
println(lengthsList)

/*
filter: The
filter: quick
filter: brown
filter: fox
filter: jumps
filter: over
filter: the
filter: lazy
filter: dog
length: 5
length: 5
length: 5
length: 4
length: 4
Lengths of first 4 words longer than 3 chars:
[5, 5, 5, 4]
*/
```

다음은 Iterable의 연산 과정 다이어그램이다. (출처: [코틀린 공식문서](https://codechacha.com/ko/kotlin-sequences/))
<img src="https://kotlinlang.org/docs/images/list-processing.png" width="90%" height="70%"></img>   
<br/>

* Sequence

```kotlin
val words = "The quick brown fox jumps over the lazy dog".split(" ")
//convert the List to a Sequence
val wordsSequence = words.asSequence()

val lengthsSequence = wordsSequence.filter { println("filter: $it"); it.length > 3 }
    .map { println("length: ${it.length}"); it.length }
    .take(4)

println("Lengths of first 4 words longer than 3 chars")
// terminal operation: obtaining the result as a List
println(lengthsSequence.toList())

/*
Lengths of first 4 words longer than 3 chars
filter: The
filter: quick
length: 5
filter: brown
length: 5
filter: fox
filter: jumps
length: 5
filter: over
length: 4
[5, 5, 5, 4]
*/
```

다음은 Sequence의 연산 과정 다이어그램이다. (출처: [코틀린 공식문서](https://codechacha.com/ko/kotlin-sequences/))
<img src="https://kotlinlang.org/docs/images/sequence-processing.png" width="90%" height="70%"></img>   
<br/>

## > 정리
Lazy evaluation으로 동작하는 Sequence는 적은 연산으로 동일한 결과를 출력할 수 있다. 특히 조건에 맞는 요소 개수가 정해져 있을 경우, Lazy evaluation으로 동작한다면 불필요한 연산을 줄일 수 있다.