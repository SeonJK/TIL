# LifeCycle
Android의 생명주기를 이해하는 것은 앱개발에서 매우 중요하다.
### why?
스마트폰은 데스크탑보다 적은 메모리를 사용하기 때문에 프로세스의 상태가 금방 변하기 때문!!

<img src="https://koenig-media.raywenderlich.com/uploads/2021/04/android-lifecycle-diagram-04-448x500.png" width="80%" height="90%"></src>

사진에 나오는 것처럼 액티비티는 정해진 순서에 따라 구동된다.   
액티비티가 처음 메모리에 올려지면 차례대로
1. onCreated()
2. onStart()
3. onResume()   
이 실행된다.   

그리고 앱이 전환이 되면
1. onPause()
2. onStop()   
이 실행되며 시스템이 메모리를 관리한다.   

다시 앱을 실행할 경우 onStart(), onResume()이 실행된다.

## > 가로화면 전환시
가로화면의 전환되게 되면 앱을 새로 구동하는 것과 같다.
따라서 액티비티는 다음과 같이 구동된다.
1. onPause()
2. onStop()
3. onDestroy()
4. onCreate()
5. onStart()
6. onResume()   

# UI 비율설정
스마트폰의 화면크기와 해상도는 매우 다양해서 맨 처음에 배우는 dp를 사용하여 UI를 구성할 경우 모든 스마트폰마다 크기를 조정해주어야 한다.   
그렇기 때문에 상용개발을 하려면 비율로 설정하여 다양한 사람들이 사용할 수 있도록 하는 것이 중요한 것 같다. 그래서 레이아웃 별로 UI 비율을 설정하는 방법을 알아보자.

## > Linear Layout
Linear Layout은 WeightSum을 사용하여 가중치 총합을 나타낸다.
```html
<LinearLayout
    ...
    android:weightSum="3"
    android:orientation="horizontal">

    <Button
        android:layout_weight="1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        />
    <Button
        android:layout_weight="1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        />
    <Button
        android:layout_weight="1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        />
</LinearLayout>
```
이렇게 작성하게 되면 화면 첫 줄에 버튼 3개가 가로로 나란히 화면을 꽉 채운 채로 구성된다.

## > Relative Layout
Relative Layout은 컴포넌트끼리의 관계를 설정해주어서 비율을 설정할 수 있다.   
### ※ Relative는 뷰끼리 겹칠 수 있다.
```html
<RelativeLayout
    ...>
    <View
        android:id="@+id/view1"

        /// 부모의 정 가운데에 위치
        android:layout_centerInParent="true"
        
        /// 부모의 특정위치에 가도록 함
        android:layout_alignParentLeft="true"
        android:layout_alignParentRight="true"

        /// 세로로 가운데, 가로로 가운데에 위치
        android:layout_centerVerical="true"
        android:layout_centerHorizontal="true"
    />

    <View
        /// 특정 컴포넌트와 관계 형성
        android:layout_toRightOf="@id/view1"
    />
</RelativeLayout>
```
## > Constraint Layout
Constraint Layout은 가로 화면으로 전환해도 비율이 유지된다.   
가로와 세로 각각 하나 이상의 Constraint를 꼭 설정해야 한다.
```html
<ConstraintLayout
    ...>

    <View
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

</ConstraintLayout>
```

# 상태바 확장
최근 앱들은 상태바를 앱의 테마와 같은 색으로 만들어 일체감을 주도록 하는게 트렌드인 것 같다. 보기에도 더 깔끔해보여서 앱에 적용해보려 한다.   

먼저 <code>theme.xml</code>에 코드를 추가해준다.
```html
    <!-- Status bar color. -->
    <item name="android:statusBarColor" tools:targetApi="l">@android:color/transparent</item>
    <!-- Status bar Text color; true->black, false->white -->
    <item name="android:windowLightStatusBar" tools:targetApi="m">false</item>
    <item name="android:enforceStatusBarContrast" tools:targetApi="q">false</item>
```
다크 모드일 때 적용한 코드로   
* 첫 번째 코드는 상태바 색상을 투명으로 만들어 주었다.
* 두 번째 코드는 상태바 글자나 상태 그림(?) 색상을 결정하는 것이다. true는 검정색, false는 흰색이 된다.
* 세 번째 코드는 공식 문서에 의하면 투명 상태바일 때 충분한 대조를 주는 코드라고 하는데 false일 때랑 true일 때 차이를 구분 못하겠다...   

그 다음 액티비티에 코드를 적용한다. 나는 Util 클래스를 만들어서 적용해보았다.   
1. 먼저 패키지 안에 Util 패키지를 생성한다.
2. Util 패키지 안에 StatusbarTransparent.kt 클래스를 생성한다.

### StatusbarTransparent.kt
```kotlin
class StatusbarTransparent {
    // 정적 메소드를 만들기 위해 companion object 사용
    companion object {
        fun Activity.setStatusBarTransparent() {
            window.apply {
                setFlags(
                    WindowManager.LayoutParams.FLAG_LAYOUT_NO_LIMITS,
                    WindowManager.LayoutParams.FLAG_LAYOUT_NO_LIMITS
                )
            }
            // API 30 이상에 적용
            if (Build.VERSION.SDK_INT >= 30) {
                WindowCompat.setDecorFitsSystemWindows(window, false)
            }
        }
    }
}
```

### MainActivity.kt
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        ...
        this.setStatusbarTransparent()
        ...
    }
}
```