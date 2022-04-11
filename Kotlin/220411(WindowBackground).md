# WindowBackground
xml 파일에서 background를 설정하게 되면 라이트 모드일 경우 흰색, 다크 모드일 경우 검은색이 나왔다가 해당 background 색상으로 변경되게 된다.   
<br/>
처음부터 해당 background 색상을 갖게 하고 싶다면 `themes.xml`에서 `windowBackground`를 설정해준다.

```html
<!-- themes.xml -->

<!-- 화면 실행 처음부터 토마토색이 나올 수 있도록 설정 -->
<item name="android:windowBackground">@color/pomodoro_red</item>
```

내가 생각하기에 Splash 화면을 설정하지 않은 경우 나오는 background 색상을 지정하는 설정인 것 같다.