# MediaRecorder

오디오 및 동영상을 인코딩하는 기능을 위해 MediaRecorder API를 사용한다.

안드로이드의 보안강화로 API 23부터 마이크와 카메라의 권한을 런타임에 사용자에게 승인을 요청해야 한다. API 28이상부터는 백드라운드에 실행하는 앱은 마이크에 액세스할 수 없다.   
<br/>

## > 상태
MediaRecorder는 7가지의 상태를 가지고 있다. (참고: [안드로이드 공식문서](https://developer.android.com/reference/android/media/MediaRecorder))

<img src="https://developer.android.com/images/mediarecorder_state_diagram.gif" height="90%" width="80%">

## > 권한 요청
오디오 권한 요청 코드는 다음과 같다.
```html
<!-- Manifest.xml -->

<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

## > 선언 및 초기화
MediaRecorder API를 사용해야할 땐 `OnRequestPermissionsResult()`메소드를 override해야 한다. 다음은 클래스 파일에서 MediaRecorder 사용방법이다.

```kotlin
// MainActivity.kt

class MainActivity : AppCompatActivity() {
    ...

    override fun OnRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<String>,
        grantResults: IntArray
    ) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResult)
        // 권한 여부 처리
    }

    val recorder: MediaRecorder? = MediaRecorder().apply {
        // 오디오 소스 설정
        setAudioSource(MediaRecorder.AudioSource.MIC)
        // 출력 파일 확장자
        setOutputFormat(Mediarecorder.OutputFormat.MPEG_4)
        // 출력 파일 이름
        setOutputFile(${externalCacheDir.absolutePath}/recordtest.m4a)
        // 인코더 설정
        setAudioEncoder(MediaRecorder.AudioEncoder.AAC)

        try {
            prepare()
            start()
        } catch (e: IOException) {
            Log.e(TAG, "prepare() failed")
        }
    }
}
```

# MediaPlayer

녹음된 오디오를 재생시키기 위해 MediaPlayer API를 사용하였다. MediaPlayer API는 저장된 미디어파일(오디오, 동영상, 이미지), 독립형 파일 또는 데이터 스트림을 모두 재생시킬 수 있다.

## > 상태
MediaPlayer는 10가지 상태를 가지고 있다. (참고: [안드로이드 공식문서](https://developer.android.com/reference/android/media/MediaPlayer))

<img src="https://developer.android.com/images/mediaplayer_state_diagram.gif" height="90%" width="80%">

## > 권한 요청
* 인터넷 권한 - 데이터 스트리밍시 필요하다.
```html
<!-- Manifest.xml -->

<uses-permission android:name="android.permission.INTERNET" />
```

* Wake Lock 권한 - 플레이어 애플리케이션셍서 화면이 어두워지는 것이나 프로세서의 절전 모드를 방지할 때 필요하다. 또는 `MediaPlayer.setScreenOnWhilePlaying()`이나 `MediaPlayer.setWakeMode()` 메소드를 사용할 때 필요하다.
```html
<!-- Manifest.xml -->

<uses-permission android:name="android.permission.WAKE_LOCK" />
```

## > 선언 및 초기화
MeidaPlayer가 지원하는 소스마다 재생방법이 다르다.   
<br/>

1. 로컬 리소스
로컬 리소스 컨텐츠는 원시 오디오가 아니여야 한다. 올바르게 인코딩되고 형식이 지정된 미디어 파일이어야 한다.
```kotlin
// create() 메소드가 prepare() 역할까지 진행
var mediaPlayer: MediaPlayer? = MediaPlayer.create(context, 리소스파일경로)
mediaPlayer?.start()
```

2. 내부 URI
```kotlin
val uri: Uri = ... // Uri 초기화
val mediaPlayer: MediaPlayer? = MediaPlayer().apply {
    try {
        setAudioStreamType(AudioManager.STREAM_MUSIC)
        setDataSource(applicationContext, uri)
        prepare()
        start()
    } catch (e: IOException) {
        Log.e(TAG, "prepare() failed")
    }
}
```

3. 외부 URL
HTTP 스트리밍을 통해 재생하는 방법
```kotlin
val url = "http://..." // URL 초기화
val mediaPlayer: MediaPlayer? = MediaPlayer(). apply {
    try {
        setAudioStreamType(AudioManager.STREAM_MUSIC)
        setDataSource(url)
        prepare()
        start()
    } catch (e: IOException) {
        Log.e(TAG, "prepare() failed")
    }
}
```

## > 비동기 준비
`prepare()`이 호출될 경우 오랜 시간이 걸릴 수 있다. 왜냐하면 미디어 데이터를 가져오고 디코딩해야 할 수 있기 때문이다. 그렇기 때문에 작업 스레드에서 준비 작업을 실행해야 한다.   
<br/>
MediaPlayer에서는 `prepareAsync()` 메소드를 지원하여 비동기 준비를 지원한다. 미디어가 백그라운드에서 준비가 완료되면 `setOnpreparedListener()`를 통해 구성된 `MediaPlayer.OnPreparedListener`의 `onPrepared()` 메소드가 호출된다.

## > MediaPlayer 해제
MediaPlayer 작업이 끝나면 항상 리소스 해제를 해주어야 한다. 단 백그라운드에서 재생할 경우는 예외이다. 해제하는 방법은 다음과 같다.

```kotlin
mediaPlayer?.release()
mediaPlayer = null
```

# AppCompat
매년 안드로이드가 새로운 버전이 나오면서 기존 버전에서는 호환성에 문제가 있을 수 있다. 이때 새로운 버전의 기능을 AppCompat을 통해 래핑하여 기존 버전에서도 사용할 수 있도록 지원한다.   
최근에는 클래스를 매핑하여 변환해주는 기능도 제공하고 있다.

# Custom View
개발자가 직접 뷰클래스를 생성하기 위해서는 다음과 같은 인자를 받아야 한다.
* Context
* AttributeSet

이미지버튼을 상속받는 RecordButton을 만들면 다음과 같다.
```kotlin
// RecordButton.kt

class RecordButton(
    context: Context,
    attrs: AttributeSet
) : AppCompatImageButton(context, attrs) {
    
    // ...
    
}
```