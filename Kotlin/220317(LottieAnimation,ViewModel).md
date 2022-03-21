# LottieAnimation
반응형 UI를 만들기 위한 방법 중 하나로 Airbnb에서 만든 애니메이션을 좋아요 버튼 예제를 통해 적용해보고자 한다. 

1. dependencies 추가   
build.gradle에서 dependencies를 추가한다.

```groovy
dependencies {
    // lottieAnimation
    def lottieVersion = '5.0.3'
    implementation "com.airbnb.android:lottie:$lottieVersion"
}
```

2. [로티 애니메이션 사이트](https://lottiefiles.com/)에 들어가 원하는 애니메이션을 다운로드 받는다.   
이 때 "Lottie JSON" 파일로 다운 받는다.

3. 프로젝트에서 New > Folder > Assets Folder를 눌러 폴더를 생성하고 다운 받은 JSON 파일을 이 폴더에 붙여넣기 한다.

4. xml 구성

```html
<com.airbnb.lottie.LottieAnimationView
        android:id="@+id/like_btn"
        /// 다운받은 json파일 이름 넣기
        app:lottie_fileName="heart.json"
        /// 액티비티가 보여졌을 때 자동실행 여부
        app:lottie_autoPlay="false"
        /// 애니메이션 반복여부
        app:lottie_loop="false" />
```

5. kotlin 작성 (좋아요 버튼 예제)
```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    var isLiked = false

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        // 버튼 클릭이벤트
        binding.likeBtn.setOnClickListener {
            
            // Custom animation speed or duration.
            // 0f = 0퍼센트, 1f = 100퍼센트
            // ofFloat(시작지점, 종료지점).setDuration(ms)
            var animator = ValueAnimator.ofFloat(0f, 0.5f).setDuration(1000)
            if (isLiked) {
                animator = ValueAnimator.ofFloat(0.5f, 1f).setDuration(500)
            }
            // 애니메이션 실행지점
            animator.addUpdateListener {
                binding.likeBtn.progress = it.animatedValue as Float
            }
            animator.start()
            isLiked = !isLiked
        }
    }
}
```

### 추가. OpenSource를 사용할 때는 공식문서를 잘 활용하는 것이 중요!!   
<br/>

# ViewModel
UI 컨트롤러에서 데이터를 처리하게 되면 UI 컨트롤러가 비대해지게 된다. 그리고 액티비티 전환이 일어날 경우 데이터가 사라질 수 있는 문제점이 있다. 그래서 ViewModel을 통해 데이터 소유권을 분리하여 효율적이고 쉬운 프로그래밍을 할 수 있다.   

## > 수명주기
<img src="https://developer.android.com/images/topic/libraries/architecture/viewmodel-lifecycle.png?hl=ko" width="80%" height="90%"></img> 

사진에 나와있는 것처럼 ViewModel은 생명주기를 고려하지 않고 어플리케이션이 동작하는동안 동일한 데이터를 사용할 수 있다.

## > 적용
1. gradle
```groovy
dependencies {
    def lifecycle_version = "2.5.0-alpha04"
    // ViewModel
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
    // LiveData
    implementation "androidx.lifecycle:lifecycle-livedata-ktx:$lifecycle_version"
}
```

2. ViewModel 클래스 생성
```kotlin
// 계산기의 기능 구분을 위한 enum class 생성
enum class ActionType {
    PLUS, MINUS
}

class MyViewModel : ViewModel() {
    private val _currentValue = MutableLiveData<Int>()

    val currentValue = LiveData<Int>
        get() = _currentValue

    init {
        _currentValue.value = 0
    }

    fun updateValue(actionType: ActionType, input: Int) {
        when(actionType) {
            ActionType.PLUS ->
                _currentValue.value = _currentValue.value?.plus(input)
            ActionType.MINUS ->
                _currentValue.value = _currentValue.value?.minus(input)
        }
    }
}
```

3. Activity에서 ViewModel 객체 생성
```kotlin
class MainActivity {
    lateinit var myViewModel: MyViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        // 뷰모델 프로바이더를 통해 뷰모델 가져오기
        // 라이프사이클을 가지고 있는 액티비티 설정. 즉 자기자신
        // 우리가 가져오고 싶은 뷰모델 클래스를 넣어서 뷰모델 호출
        myViewModel = ViewModelProvider(this).get(MyViewModel::class.java)

        // 뷰모델이 가지고 있는 값의 변경사항을 관찰할 수 있는 라이브 데이터를 옵저빙한다.
        myViewModel.currentValue.observe(this, Observer {
            // 옵저빙할 컴포넌트 기능
        })
    }
}
```