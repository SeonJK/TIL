# 오늘 내가 만난 에러들
## > UninitializedPropertyAccessException
원인   
- lateinit 변수를 초기화하지 않고 사용할 때 발생한다.
- 배열을 초기화하는 방법을 모른다.

<p>해결방법</p>

1. 내부에서 제공하는 API를 사용
    * 배열을 선언과 동시에 초기화할 때 -> arrayOf(value)
    * 나중에 초기화할 때 -> arrayOfNulls(size)
    ```kotlin
    val arr1: Array<Int> = arrayOf(1,2,3)
    val arr2 = arrayOfNulls<Int>(3)

    arr1.forEach{ print("$it ")}    // 1 2 3
    arr2.forEach{ print("$it ")}    // null null null
    ```
2. 생성자를 이용
    * Array 생성자는 Array(size, 람다식)으로 구성되어있다.
    ```kotlin
    // 람다식 { 좌측: index, 우측: value }      // i는 0부터 시작
    val array: Array<Int> = Array(3) { i -> i+1}

    array.forEach{ print("$it ")}   // 1 2 3
    ```

# Collections
Kotlin에서 Collections는 3가지로 구성되어있다.
1. List
    - 순서를 지키는 Collection 타입
    ```kotlin
    val numbers = listOf(1,2,3,4)
    val numbers = mutableListOf(1,2,3,4)
    ```
2. Set
    - 집합과 같은 개념
    - 동일한 value가 존재할 수 없는 Collection 타입
    ```kotlin
    val numbers = setOf(1,2,3,4)
    val numbers = mutableSetOf(1,2,3,4)
    ```
3. Map
    - 1:1 관계(Key:Value)
    ```kotlin
    val numbersMap = mapOf("key1" to 1, "key2" to 2, "key3" to 3, "key4" to 4)
    val numbersMap = mutableMapOf("key1" to 1, "key2" to 2, "key3" to 3, "key4" to 4)
    ```

이외에도 Collections의 다이어그램은 다음과 같다. (출처: [코틀린 공식문서](https://kotlinlang.org/docs/collections-overview.html#collection-types))
<img src="https://kotlinlang.org/docs/images/collections-diagram.png" width="80%" height="80%"></img>

컬렉션에 다양한 명령어가 있으므로 공식문서를 참조하며 개발하는게 중요할듯 하다.