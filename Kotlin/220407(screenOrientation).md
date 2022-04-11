# 화면모드 설정
액티비티의 화면을 고정하려고 할 때 2가지 방법을 사용할 수 있다. 메니페스트에서 설정하는 것과 Programmatic하게 설정하는 방법이 있다.

1. AndroidManifest.xml
```html
<activity
    android:name="MyActivity"
    /// 세로화면 고정
    android:screenOrientation="portrait"></activity>
    /// 가로화면 고정
    android:screenOrientation="landscape"></activity>
```

2. Programmatic code
```kotlin
override fun onCreate( ... ) {
    super.onCreate(savedInstanceState)

    setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE)

    setContentView(R.layout.main)
}
```

# 전체화면 설정

# timer