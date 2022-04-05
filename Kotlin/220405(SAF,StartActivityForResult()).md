# Storage Access Framework
학부 때 안드로이드를 할 때만 하더라도 유저에게 저장공간 접근권한을 받고 저장공간을 선택해서 이미지를 불러왔었는데 프로젝트를 하면서 Storage Access Framework(이하 SAF)라는 개념을 배우게 되어 정리해보고자 한다.

## > 개요
SAF는 문서 및 이미지 등 각종 파일을 탐색하고 저장하는 작업을 간편하게 하려고 도입되었다. SAF는 프레임워크이기 때문에 다음과 같은 항목을 포함하고 있다.
* Document Provider: 문서(파일)을 제공하는 객체(ex: GoogleDrive). `DocumentsProvider`의 서브클래스이다.
* Client app: Provider가 제공하는 문서를 사용하는 앱. `ACTION_CREATE_DOCUMENT`, `ACTION_OPEN_DOCUMENT`, `ACTION_OPEN_DOCUMENT_TREE` 등의 인텐트 작업을 호출하고 Provider에서 반환한 파일을 수신한다.
* Selector: 일종의 시스템 UI로, Client app에서 필요한 파일을 사용자가 선택할 때 사용한다.

## > 특징
* 디바이스에는 여러 Provider가 존재할 수 있다.
* 유저는 Provider들이 제공하는 모든 파일을 탐색할 수 있다.
* 앱은 Provider가 제공하는 문서에 대한 장기적, 지속적 접근 권한을 가질 수 있다. -> 권한마다 파일을 추가, 편집, 저장 및 삭제할 수 있다.
* USB가 연결되었을 때 USB의 데이터를 제공하는 Provider도 지원한다.
* 권한을 요구하지 않아도 된다. 대신 Selector UI를 띄워 파일에 접근한다.

SAF를 사용하는 예제코드는 [공식문서 링크](https://developer.android.com/training/data-storage/shared/documents-files?hl=ko)를 참고하면 된다.


# startActivityForResult()
startActivityForResult()가 Deprecated되었다. Deprecated된 이유는 다음과 같다.
* Androidx Activity와 Fragment에 도입된 Activity Result API 사용을 적극 권장하고 있다.
* 결과를 얻기 위한 로직을 실행할 때, 메모리 부족으로 인해 프로세스와 활동이 소멸될 수 있다.

`startActivityResult()`와 `onActivityResult()`를 같은 곳에 구현하는 로직때문에 Activity가 메모리 부족으로 사라질 경우 Uri를 받아와도 기능을 실행하지 못하는 문제인 것이다.   
그래서 Activity를 실행하는 부분과 Result CallBack부분을 분리해서 만들어주어야 하는데 이를 위해 ActivityResultLauncher 객체와 `registerForActivityResult()`가 사용되게 된다.   
<br/>
`registerForActivityResult()`는 콜백을 등록하는 역할을 한다. Result CallBack부분을 분리하여 구현하기 때문에 REQUEST_CODE도 필요하지 않다.   
이와 같이 구현하게 되면 다음과 같이 로직이 동작한다.

1. 액티비티A -> 액티비티B 실행, 메모리가 부족해서 A 소멸
2. 액티비티B 종료 후 `setResult()`메소드로 결과값 넘김
3. A가 다시 생성되어도 `registerForActivityResult()`메소드가 다시 콜백을 등록하기 때문에 결과값을 받을 수 있음

코드는 [공식문서](https://developer.android.com/training/basics/intents/result?hl=ko)를 참조하자.
