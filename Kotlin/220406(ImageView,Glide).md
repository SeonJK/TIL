# ImageView에 이미지 출력하기
ImageView에서 이미지를 출력하는 방법 중 자주 쓰이는 내부 API 5가지 방법의 차이를 정리해보고자 한다.

## 1. setImageResource(resid: Int)
* 프로젝트 내의 res/drawable/ 폴더의 이미지를 load하는 함수

## 2. setImageUri(uri: Uri?)
* Uri에 이미지 파일 경로를 parse하여 이미지를 load하는 함수

## 3. setImageBitmap(bitmap: Bitmap)
* 비트맵을 load하는 함수
* 비트맵이란 이진 배열 방식을 사용하는 그래픽 개념이다.
* 안드로이드에서는 비트맵 파일로 `.png`, `.jpg`, `.gif` 형식을 지원한다.

## 4. setImageDrawable(drawable: Drawable)
* Drawable 형식의 이미지를 load하는 함수
* Drawable이란 화면에 그릴 수 있는 그래픽에 대한 일반적인 개념이다.   
다양한 유형이 있다. (`BitmapDrawable`, `NinePatchDrawable`, `LayerDrawable` 등)

## 5. setImageIcon(icon: Icon?)
* 명시된 아이콘을 load하는 함수
* UI 스레드에 보여지는 비트맵을 가져오는 것이 좋다. 

## > Glide
내부 API 외에도 Glide를 통하여 이미지를 로딩할 수 있다.   
Glide는 이미지를 빠르고 효율적으로 불러올 수 있게 도와주는 라이브러리이다. 사용방법이 간단해서 많이 사용하는 라이브러리인 것 같다.   
이미지 뿐만 아니라 GIF, 비디오 스틸의 로딩과 디코딩, 캐싱 등 다양한 API를 사용할 수 있다.   
기본적으로 `HttpUrlConnection`, `Volley`나 `OkHttp`라이브러리를 사용할 수 있는 플러그인도 지원한다.

* 사용법   
1. 먼저 dependency를 추가해준다.
    ```groovy
    dependencies{
        implementation 'com.github.bumptech.glide:glide:$glide-version'
        annotationProcessor 'com.github.bumptech.glide:compiler:$glide-version'
    }
    ```

2. 단순히 뷰에 이미지를 넣는 작업은 이러하다.
    ```kotlin
    Glide.with(this)    // this: Activity
        .load(uri)    // load할 이미지 주소 혹은 resid값
        .into(imageView)
    ```
    굉장히 간단하게 이미지를 넣을 수 있는 것을 볼 수 있다. 하지만 Glide는 에러 상황이 발생하거나 후가공이 필요할 때 손쉽게 처리하는 상황에서 빛을 발할 수 있다.   
    <br/>
    먼저 이미지 로딩 전/후처리에 대한 함수이다.
    ```kotlin
    Glide.with(this)
        .load(uri)
        .placeholder(R.drawable.img_placeholder)    // 이미지 로딩을 시작하기 전에 보여줄 이미지
        .error(R.drawable.img_error)    // 리소스 로딩 중 에러가 발생했을 때 보여줄 이미지
        .fallback(R.drawable.img_no_img)    // load할 uri가 null일 경우 보여줄 이미지
        .into(imageView)
    ```

    다음은 캐싱에 대한 함수이다.
    ```kotlin
    Glide.with(this)
        .load(uri)
        .skipMemoryCache(true)  // 메모리에 캐싱하지 않으려면 true로 설정
        .diskCacheStrategy(DiskCacheStrategy.NONE) // 디스크에 캐싱하지 않을 경우 NONE으로 설정
        // NONE 외에도 ALL, AUTOMATIC, DATA, RESOURCE 옵션이 있다.
        .into(imageView)
    ```