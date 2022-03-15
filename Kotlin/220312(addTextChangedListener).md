# addTextChangedListener
editText를 사용할 때 필요한 Listener이다. 여기서는 3가지의 메소드를 오버라이드 해야한다.
* beforeTextChanged()<br/>
텍스트를 입력하기 전에 새 텍스트로 대체될 것임을 알리기 위해 호출되는 메소드
* onTextChanged()<br/>
editText에  호출되는 메소드
* afterTextChanged()<br/>
입력이 끝나고 나서 다른 곳에 통보할 때 사용되는 메소드

```kotlin
editText.addTextChangedListener(object: TextWatcher {
    // 텍스트를 입력하기 전
    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
        // s: CharSequence? = 현재 editText에 입력된 값
        // start: Int = s에서 새로 추가될 문자열의 시작 위치 값
        // count: Int = s에 새로운 문자열이 추가된 후 문자열의 길이
        // after: Int = 새로 추가될 문자열의 길이
    }
    
    // editText가 변화가 있을 때
    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
        // s: CharSequence? = 새로 입력한 텍스트를 포함한 editText값
        // start: Int = 새로 추가된 문자열의 시작 위치 값
        // before: Int = 입력되기 전 기존 문자열의 길이
        // count: Int = 새로 추가된 문자열의 길이
    }

    // 입력이 끝났을 때
    override fun afterTextChanged(p0: Editable?){
        // p0: Editable? = editText가 변경됐음을 알림
    }
})
```