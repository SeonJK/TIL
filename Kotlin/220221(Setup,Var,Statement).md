# Kotlin
2017년 Android 개발을 위한 공식 언어로 지정된 언어이다. 채용 공고를 보더라도 앱 개발분야에서 Java 개발 경험보다 Kotlin 개발 경험이 있는 지원자를 원한다.<br/>

Kotlin의 특징
* NullPointerException이 없다.<br/>
Java개발해본 사람은 미치도록 공감할 가장 두려운 Error일 것 같다. 이 Error가 없다는게 가장 강력한 이유인 것 같다.
* 표현이 매우 간결하다.
* Coroutine을 사용하여 비동기 코드를 쉽게 사용할 수 있다고 한다.
* 연산자 오버로딩에서 많은 개선이 있다.
---

# Kotlin 개발환경 세팅
## > Android Studio VS IntelliJ
- Android를 개발할 때 당연히 Android Studio가 편하다고 생각되는 건 Android를 모르는 사람이 봐도 아는 사실이다. <br/> 그럼에도 내가 두 가지를 놓고 고민하는 이유는 현재 나는 Eclipse를 이용하여 **일반 JAVA**를 공부하고 있기 때문이다. IntelliJ를 활용하여 Android 개발이 가능하다는 글을 구글링하다 제목만 스쳐지나가며 봤기 때문에 하나의 IDE로 두 가지 개발이 가능한지 궁금했다.

  ### 결론
  ![logo](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/IntelliJ_IDEA_Icon.svg/1200px-IntelliJ_IDEA_Icon.svg.png)
  - 나는 어플 배포까지 하지 않고 NDK 사용은 계획에 없기 때문에 **IntelliJ**로 같이 개발해보도록 하겠다.

  ### IntelliJ
  - Android Studio의 뿌리이다.
  - Gradle과 Maven 빌드 시스템 지원이 좋다.
  - SDK를 따로 다운받아서 설정해야 한다.
  - Android 관련 플러그인을 모두 활성화하면 프로그램이 무거워진다.(Android Studio도 무거운건 매한가지...)
  - 플레이 스토어 등록을 지원하지 않는다.
  - 업데이트가 Android Studio에 비해 느리다.

  ### Android Studio
  - IntelliJ를 기반으로 하고 있다.
  - Gradle 빌드 시스템이 잘 되어있다.
  - 에뮬레이터를 다시 시작없이 변경사항을 푸시할 수 있다.
  - 다양한 테스트 도구와 Framework를 지원한다.
  - C++ 및 NDK를 지원한다.
  - 플레이 스토어 등록을 지원한다.
  - 업데이트가 IntelliJ에 비해 빠르다.
  - 오류가 많은 편이다.
  - 무척 무겁다..(램과 CPU 차지 비율이 ㅎㄷㄷ)

---

## > JDK 설치
JDK는 이미 JAVA를 공부하면서 설치했기 때문에 이번에 따로 설치하지 않는다.
## > IntelliJ 설치
1. IntelliJ 사이트에 들어가서 무료버전인 Community 버전을 설치한다.
2. Setup 과정에서 Next만 눌러줘도 상관없으니 설명 Pass
3. New Project 생성
4. Android 탭에서 "Install SDK"를 눌러 설치를 진행한다.
5. 설치가 완료되면 "Next"를 눌러 프로젝트를 생성한다. 이때, 언어는 Kotlin으로 설정하기!!

---

# 함수
- Kotlin은 JAVA와 다른 형식을 가지고 있다.

```kotlin
fun helloWorld(): Unit { //Unit == void
    //Unit은 생략가능
}

fun add(a: Int, b: Int): Int {
    //자료형은 Capital로 시작
    return a+b
}
```
* ※ 갑자기 이런 에러가 나왔다.
  * Execution failed for task ':app:processDebugAndroidTestManifest'.
  * 해결<br/>
  gradle에서 compilSdk와 minSdk의 차이가 너무 커서 발생하는 듯하다. (설정 - compilSdk: 32, minSdk: 24)<br/>
  ⇒ compileSdk: 30으로 설정

# val VS var
> * val(value) == 상수<br/>
> * var(variable) == 변수

```kotlin
val a: Int = 10 //정수형 상수 선언 및 초기화
var b: Int = 9  //정수형 변수 선언 및 초기화

var name: String = "Seon"   //문자열 변수 선언 및 초기화
```
### Kotlin은 자료형을 명시하지 않아도 알아서 적합한 자료형을 할당한다.

```kotlin
val a = 10 //정수형 상수
var b = 9 // 정수형 변수

a = 100 //Compile Error
b = 100

var name = "Seon"
```

# Nullable
### Kotlin은 기본적으로 Null값을 가지지 못하게 한다.(∵NullPointerException) <br/> null이 꼭 필요한 경우 ***nullable***로 지정한다. !!!정말 신중하게 처리할 것!!!<br/>

```kotlin
val languageName: String? = null
```

# String Template
### Kotlin은 문자열 안에 변수를 편리하게 사용할 수 있다.

> "문자ㅏㅏㅏㅏㅏ ${변수} ㅏㅏㅏ"
```kotlin
val name = "JK"
val lastName = "Seon"

//문자열 안에서 $표시만 해주면 변수로 인식한다.
println("my name is $name I'm a Developer.")
//여러 변수를 조합하거나 문자열출력시 문자를 변수에 붙여야할 때 {}로 구분하여 사용한다.
println("my name is ${name + lastName}. I'm a Developer.")

println("is it true? ${1==0}") //false
```

# 조건문
## 1. if문
  * 기존 JAVA와 형식이 같음
    * if
    * if - else
    * if - else if - else

```kotlin
if(조건식) {
    조건==참일 경우 실행문
}else { 
    조건==거짓일 경우 실행문 
}
```

* 기존 문장에 비해 획기적으로 쉽게 적을 수 있는 문법도 존재 (삼항 연산자랑 비슷)
  
```kotlin
fun maxNum(a : Int, b : Int) = if(a>b) a else b
// reutrn타입은 알아서 지정
```

### 2. when문
* if문의 조건이 복잡해지면 when 표현식을 사용할 수 있다.

```kotlin
when(조건){
    조건 1 -> 수행문
    조건 2 -> 수행문
    else -> 수행문
}
```

```kotlin
when(score){    //범위 연산자인 "in .."을 사용하여 간단하게 범위 지정 가능
    in 90..100 -> println("A")
    in 70..80 -> println("B")
    else -> println("fail")
}
```

### 3. Expression VS Statement
>- Expression: 다른 언어에서 흔히 봤던 문장들. ex> if문, switch문 등 return값이 불필요<br/>
>- Statement: return값을 반환해줘야 하는 문장. 배정문이 활용될 때 사용

```kotlin
//Expression
when(score){
    in 90..100 -> println("You are genius")
    in 10..80 -> println("not bad")
    else -> println("okay") //필수 X
        }
```
```kotlin
//Statement
var b = when(score){
    1 -> 1
    2 -> 2
    else -> 3   //필수
}
```