# findViewById()
<p>JAVA 안드로이드에서는 xml 요소들을 class 파일과 연결시킬 때 findViewById()를 사용하여야 했다. 이 함수는 각 요소마다 적절한 타입을 지정해줘야 한다는 불편함이 있었는데...
<p>Kotlin에서는 이러한 불편함을 하나의 코드로 해결할 수 있다!!! (겁나 편함)<br/>
build.gradle에서 plugins에 한줄만 추가해주면 된다!

```groovy
plugins{
    id 'kotlin-android-extensions'
}
```

이렇게 추가하고 나면 별도로 findViewById()를 사용하지 않고 바로 xml의 id를 입력해주면 자동 import가 진행되어 코딩을 더욱 빠르게 도와준다.
<p> extension을 쓰면 좋은 점은 3가지가 있다.

1. 타입문제 해결<br/>
위에서 이야기 한 것처럼 타입을 지정할 필요없이 자동으로 import된다.
2. Null 체킹<br/>
JAVA의 고질적인 문제인 NPE를 체크하고 피할 수 있다. kotlin은 기본적으로 null을 허용하지 않지만 [?]를 사용하면 null을 사용할 수 있다.
```kotlin
textView?.text = ""
```
3. 코드 간결화
<br/><br/>

?? 궁금한 점 ??
* ViewBinding도 알아볼 것.