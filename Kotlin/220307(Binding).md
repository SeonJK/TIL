# ViewBinding
어제 공부했었던 kotlin-extensions는 21년에 지원이 중단되었다고 한다...! ㅠㅠㅠㅠ
<p>왜냐하면 다음과 같은 문제가 있기 때문이다.

* 전역 네임스페이스가 오염된다.
* 개발자가 실수로 다른 레이아웃의 동일한 id를 가진 뷰를 가져오면서 NPE가 발생할 가능성이 있다.
* Kotlin만 지원이 가능하다.
<p> 그래서 ViewBinding을 통해서 뷰의 id를 참조할 수 있다. (JAVA와 Kotlin 모두 가능)

## > 특징
ViewBinding을 설정하게 되면 크게 2가지가 변화한다.<br/>

1. xml파일 이름과 뷰의 id가 파스칼 케이스(첫 문자가 대문자) + 카멜 케이스(단어 단위로 대소문자 처리)로 변화
2. xml파일 이름 끝에 Binding이라는 명칭이 추가

## > 사용법
1. gradle 추가
```groovy
android{
    viewBinding{
        enabled = true
    }
}
```
2. Activity
```kotlin
class MainActivity : AppCompatActivity() {
    
    //xml 파일 명에 따라서 "Activity이름Binding"
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.textview.text = "안녕"
    }
}
```
2. -1) Fragment
```kotlin
class MainFragment : Fragment() {
    
    // xml 파일 명에 따라서 "Fragment이름Binding"
    private lateinit var binding: FragmentMainBinding

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        binding = FragmentMainBinding.inflate(inflater, container, false)
        binding.textview.text = "안녕"
        return binding.root
    }

    override fun onDestroyView() {
        super.onDestroyView()
        binding = null
    }
}
``` 
Fragment에서는 Activity와는 다르게 onDestroyView() 메소드에서 
```kotlin
binding = null
```
넣어준다. 왜일까?
<p> 공식문서에는 이렇게 적혀있다.

> Fragments outlive their views. Make sure you clean up any references to the binding class instance in the fragment's onDestroyView() method.

Fragment는 View보다 더 오래 살아남는다고 한다. 그래서 Binding Class는 View에 대한 참조를 가지고 있는데 View가 제거될 때(= onDestroyView) 이 Binding Class의 인스턴스도 같이 정리해주는 것이다.

3. viewBindingIgnore

공식문서에 viewBindingIgnore 속성을 사용하면 Binding Class를 생성하지 않는다고 나와있다.
<p>Binding Class는 Layout 별로 무조건 생성되는데 이 속성을 통해 해당 Layout은 Binding Class를 생성하지 않도록 할 수 있다.

```html
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://shcemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    /// 뷰 바인딩 클래스 생성을 안하고 싶을 때
    tools:viewBindingIgnore="true"
    tools:context=".MainActivity">

</androidx.constraintlyout.widget.ConstraintLayout>
```

# DataBinding
ViewBinding을 구글링하다가 DataBinding이라는 단어도 보게 되었다. 둘다 이름만 봐서는 Binding하는 것인데 무엇이 다른걸까?
## > ViewBinding vs DataBinding

기본적으로 DataBinding은 ViewBinding의 역할을 할 수 있다.
* 속도의 차이 (ViewBinding이 더 빠르다.)
* DataBinding은 \<layout> 태그를 사용하여 만든 레이아웃을 처리하고, TAG를 삽입한다.
* ViewBinding은 단방향 바인딩을 지원한다. -> DataBinding은 양방향 바인딩을 지원한다.

### 결론
DataBinding이 모든 역할을 할 수 있지만, 단순히 findViewById()를 대체하기 위한 것이라면 ViewBinding을 사용할 것을 권장하고 있다.(∵ 속도가 더 빠르고 용량 절약)

## > DataBinding 사용법
1. gradle 추가
```groovy
android{
    dataBinding{
        // 데이터바인딩 활성화
        enabled = true
    }
}
```
2. Activity
```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)

        binding.textView.text = "안녕"
    }
}
```

3. XML
```html
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <!-- layout에서 사용할 객체 생성 -->
    <data>
        <variable
            name=""

        >
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

        <TextView
            android:id="@+id/textView"
            andorid:lyout_width="wrap_content"
            andorid:lyout_height="wrap_content"
            android:text="@{data." />
        
    </androidx.constraintlayout.widget.ConstraintLayout>

</layout>
```
