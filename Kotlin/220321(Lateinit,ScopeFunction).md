# 지연생성
## > 필요성
지연생성은 NullSafe를 중시하는 코틀린에서 중요한 개념이다. 변수를 Nullable로 설정해 Null값을 저장할 수 있지만 Nullable을 자주 사용하는 것은 권고하지 않는다. (그리고 변수를 사용할 때마다 Nullable타입을 작성해주어야해서 귀찮다.) 
또한 객체를 생성하고 나중에 초기화가 필요한 경우에도 사용한다.

## > 방법
1. lateinit   
    * NullSafe 변수를 lateinit 키워드로 생성한다. 이때 변수는 var로 선언한다.   
    * lateinit 키워드를 사용한 변수를 초기화하지 않으면 UninitializedPropertyAccessException Fatal로그가 남는다.
    * lateinit 키워드를 사용한 변수는 Kotlin에서 자동으로 생성해주는 getter/setter를 사용하지 못하고 custom도 못한다.
    ```kotlin
    // viewBinding구현 시 모든 함수에 적용할 수 있도록 전역변수 선언

    lateinit var binding: ActivityMainBinding

    onCreate( ... ) {
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
    }
    ```

2. lazy init
    * lazy 함수를 사용할 때 변수는 val로 선언한다(읽기 전용 프로퍼티가 구현). by 키워드(위임 프로퍼티)를 통해 lazy 함수에 객체 생성을 위임한다.
    * ***{중요}*** lazy init을 한 뒤에 사용하지 않으면 초기화가 이루어지지 않는다
    * lazy 함수는 기본적으로 쓰레드에 안전하다.
    * 필요에 따라 동기화에 사용할 Lock을 lazy 함수에 전달할 수 있다. 다중 쓰레드 환경에서 사용하지 않는 프로퍼티를 위해 lazy함수가 동기화를 하지 못하게 막을 수 있다.
    * lazy 함수를 사용한 변수는 Kotlin에서 자동으로 생성해주는 getter를 사용할 수 있지만 setter는 사용할 수 없다. getter/setter를 custom은 가능하다.
    ```kotlin
    // Livedata 구현

    private val users: MutableLiveData<List<User>> by lazy {
        // also = 객체를 리턴한다. 
        MutableLiveData<List<User>>().also {
            loadUsers()
        }
    }
    ```


# Scope Function
람다함수를 사용할 때 많이 나오는 키워드들이다. 해당 범위에서 어떤 return 값을 쓰고, 그 return값을 정의하지 않으면 어떻게 참조되는지 익혀둘 필요가 있다.   
**[자바와 다른점]** 범위함수를 사용한 람다식에서는 객체 프로퍼티에 접근할 수 있으므로 this 프로퍼티를 생략할 수 있다.

|Function|Object reference|Return value|Is extension function|   
|:---:|:---:|:---:|:---:|
|<code>let</code>|<code>it</code>|Lambda result|Yes|
|<code>run</code>|<code>this</code>|Lambda result|Yes|
|<code>run</code>|-|Lambda result|No: called without the context object|
|<code>with</code>|<code>this</code>|Lambda result|No: takes the context object as an argument|
|<code>apply</code>|<code>this</code>|Context object|Yes|
|<code>also</code>|<code>it</code>|Context object|Yes|

* let 함수
    * 수행했던 결과 값이 리턴 -> 람다식의 수행결과가 중요할 때 사용
    * 주로 nullable 변수에 값을 저장할 때 사용
* run 함수
    * 주로 객체 구성과 결과 계산이 한번에 있을 때 사용
* with 함수
    * 주로 객체를 통해 무엇을 할 수 있는지 보여줄 때 사용
* apply 함수
    * 주로 객체 초기화에 사용
* also 함수
    * 주로 객체 유효성이나 디버깅의 용도로 사용
    * 객체가 매개변수로 전달이 된다 -> 매개변수를 따로 선언하지 않으면 it으로 접근가능
