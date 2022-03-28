# SharedPreference

SharedPreference는 주로 간단한 값을 저장할 때 사용한다. 예를 들어 초기 설정 값이나 자동 로그인 여부 등 옵션 값을 많이 저장한다.   
저장할 때는 어플리케이션에 파일 형태도 저장되며, 데이터는 (key, value) 형태로 "data/data/패키지명/shared_prefs" 폴더 안에 xml 파일로 저장된다.  
<br/>
SharedPreference를 사용할 때는 앱에서 전역적으로 사용하기 때문에 싱글톤 패턴을 사용하여 만드는 것이 좋다.

## > 사용법
여기서는 FastCampusProject 중 SecretDiary를 제작한 예제를 가져오겠다.   

1. App.kt
```kotlin
import android.app.Application

class App : Application() {

    companion object {
        lateinit var prefs : MySharedPreferences
    }

    override fun onCreate() {
        prefs = MySharedPreferences(applicationContext)
        super.onCreate()
    }
}
```

2. MySharedPreferences.kt

```kotlin
import android.content.Context
import android.content.SharedPreferences

class MySharedPreferences(context: Context) {

    private val prefsFilename = "prefs"
    private val prefsKeyPassword = "diaryPassword"
    private val prefsKeyContent = "diaryText"
    private val prefs: SharedPreferences = context.getSharedPreferences(prefsFilename, 0)

    var diaryPassword: String?
        get() = prefs.getString(prefsKeyPassword, "000")
        set(value) = prefs.edit().putString(prefsKeyPassword, value).apply()

    var diaryText: String?
        get() = prefs.getString(diaryText, "")
        set(value) = prefs.edit().putString(prefsKeyContent, value).apply()
}
```

## > commit과 apply   

* commit은 UI Thread를 잠시 블록하고 내용을 저장한다.
* apply는 기다리지 않고 비동기로 저장한다.   
<br/>

# AlertDialog
경고 메시지를 확실히 띄울 때(?) 쓰는 것 같다. AlertDialog는 Builder를 사용하여 만든다. AlertDialog는 세 가지 버튼을 만들 수 있다.(Neutral, Negative, Positive)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzDOLs%2Fbtq4rdnnluK%2FshffQkhGsw1FJ9aDkzkyM1%2Fimg.png" hegiht="70%" width="90%"> </img>   
(출처: [https://black-jin0427.tistory.com/357](https://black-jin0427.tistory.com/357))   
<br/>

```kotlin
AlertDialog.Builder(this)
    .setTitle("제목")
    .setMessage("메시지내용")
    .setPositiveButton("버튼 텍스트") { dialog, which ->
        // 버튼 클릭 시 기능 작성
    }
    .setNegativeButton("버튼 텍스트") { dialog, which ->
        // 버튼 클릭 시 기능 작성
    }
    .setNeutralButton("버튼 텍스트") { dialog, which ->
        // 버튼 클릭 시 기능 작성
    }
    .create()
    .show()
```

# Handler
스레드를 관리할 때 메인스레드(UI Thread)와 새로운 스레드를 연결이 필요할 때가 있다. 왜냐하면 메인 스레드가 아닌 곳에서는 UI change를 하는 동작을 할 수 없기 때문이다.   
Handler는 메인스레드와 새로운 스레드의 연결을 위해 안드로이드가 제공하는 기능이다.

## > 사용법
1. 핸들러 선언 및 초기화
```kotlin
// MainLooper - 메인스레드와 연결된 핸들러 생성
val handler = Handler(Looper.getMainLooper())
```

2. 스레드와 핸들러 
```kotlin
override fun onCreate(...) {
    ...

    // 잠깐 멈출 때마다 수행할 스레드 설정
    val runnable = Runnable {
        // 지속적으로 수행할 기능 작성
    }

    binding.diaryContent.addTextChangedListener {
        // 핸들러 설정 전에 pending 되어있는 Runnable을 지워주기 위해 사용
        handler.removeCallbacks(runnable)
        // 0.5초 이후에 스레드(Runnable)을 실행시키도록 함
        handler.postDelayed(runnable, 500)
    }
}
```