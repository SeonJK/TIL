# 배열과 리스트
## > 배열
* 정적 할당 → 선언 시 크기를 지정해야 함
* element의 수정이 가능
```kotlin
val array: Array<Int> = arrayOf(1,2,3)
val array2 = arrayOf(1,"d",3.4f)        //Array<Any>로 자동타입추론

array[0] = 3
array.set(1, 5) //set(index, value)
array[1]        //5
array.get(0)    //3
```

## > List
### List는 수정이 불가능한 List와 수정이 가능한 mutableList로 나뉜다.
* List(수정 불가능)
  * get(), contains() 등의 함수가 쓰임
  ```kotlin
  val list: List<Int> = listOf(1,2,3)
  val list2 = listOf(1,"d",11L) //List<Any>로 자동타입추론

  list[0] = 2   //수정 불가능하기 때문에 Error 발생
  var result = list.get(0)
  ```

* mutableList(수정 가능)
  * add(), remove(), set(), clear() 등의 함수가 쓰임
```kotlin
val arrayList = arrayListOf<Int>()
arrayList.add(10)
arrayList.add(20)
//arrayList의 element를 추가하더라도 주소가 변경되는 것이 아니기 때무에 val을 써도 element 추가가능

arrayList = arrayListOf()   //val cannot be reassigned
```

?? 궁금한 점 ??
* arrayList는 mutbaleList의 종류인가?
* 배열은 java의 동적할당처럼 할당하는 방법은 없는가?

# 반복문
## > for - python문법과 비슷
```kotlin
for(반복지정자 in List){
    반복수행문
}
```
```kotlin
val students = arrayListOf("james", "ben", "mason")
for(name in stduents){
    println("${name} ")
}
//james ben mason

for(i in 1..10) {}          //1부터 10까지 반복
for(i in 1..10 step 2) {}   //1부터 10까지 2간격으로 반복
for(i in 10 downTo 1) {}    //10부터 하나씩 내려오며 반복
for(i in 1 until 10) {}     //1부터 10미만까지 반복

for((index, name) in students.withIndex()){
    println("${index+1}번째 학생: ${name}")
}
/*
1번째 학생: james
2번째 학생: ben
3번째 학생: mason
*/
```

## > while
```kotlin
var index = 0
while(index < students.size){
    println("${index+1}번째 학생: ${students[index]}")
    index++
}
/*
1번째 학생: james
2번째 학생: ben
3번째 학생: mason
*/
```

# Nullable과 NonNull
어제 배운거에 이어서
```kotlin
val nullName: String? = null    //Nullable, 타입 생략불가
val nullNameInUpperCase = nullName?.uppercase() //null이면 null반환, 아니면 uppercase() 실행
```
## > ?: → 앨비스 연산자, null일 경우 반환할 값을 지정하는 연산자
```kotlin
val lastName: String? = null
val fullName = name+" "+(lastName?: "No lastName")
//lastName이  null일 경우 "No lastName"출력, null이 아니면 lastName값을 출력
```

## > !! → null이 들어가지 않음을 컴퓨터에게 명시해주는 연산자
```kotlin
val mNotNull: String = str!!    //타입 명시는 자유
val upper = mNotNull.uppercase()
println(upper)

//email 변수가 null일 경우 let함수 안은 수행X,
//null이 아닐경우 "my email is tjswhdrbs@naver.com" 출력
val email: String? = null
email?.let{
    println("my email is ${email})
}
```

# 클래스
Kotlin에서 클래스 이름은 JAVA와 달리 파일 명과 달라도 생성이 가능하다.
## > 생성자

```kotlin
class Human constructor(val name: String = "Anonymous"){    //주 생성자(constructor) 정의
    init{   ...   }  //클래스가 호출될 때 가장 처음 할 행동 정의
    constructor(name: String, age: Int): this(name){    ...    }       //부 생성자 정의
    //this()를 써서 주 생성자를 받아와야 부 생성자 정의 가능
}

fun main(){
    //Kotlin은 new 연산자 안써도 됨
    val human = Human()             //주 생성자의 param이 이미 데이터가 있기 때문에 기본 생성자 사용 가능
    val human2 = Human("james")     //주 생성자 사용
    val human3 = Huma("joyce", 25)  //부 생성장 사용

    println("this human's name is ${human.name}")   //Anonymous
    println("this human's name is ${human2.name}")  //james
    println("this human's name is ${human3.name}")  //joyce
}
```

## > 상속
기본적으로 class와 함수 등은 final이다. 그래서 상속이나 오버라이딩을 하기 위해서는 접근지시자를 바꿔주어야 한다.
> open
```kotlin
open class Human(val name: String = "Anonymous"){
    open fun sing(){
        println("lalala")
    }
}

class Korean: Human(){
    override fun sing(){
        super.sing()
        println("라라라")
    }
}

fun main(){
    val kHuman = Korean()
    kHuman.sing()
}
/*
lalala
라라라
*/
```

?? 궁금한 점 ??
* 빈칸이 가능한 주 생성자를 만들고 이 클래스를 상속받아서 param을 바꿔서 사용하려고 하는데 null이 출력된다. null이 안나오게 어떻게 하지? Ex>
```kotlin
open class Human(open val name: String = "Anonymous"){
    init{
        println("${name} has been born!")
    }
    open fun sing(){
        println("lalala")
    }
}

class Korean(override val name: String): Human(){
    override fun sing(){
        super.sing()
        println("라라라")
        println("my name is ${name}")
    }
}

fun main(){
    val kHuman = Korean("Ben")
    kHuman.sing()
    /*
    null has been born!
    lalala
    라라라
    my name is Ben
    */
}
```