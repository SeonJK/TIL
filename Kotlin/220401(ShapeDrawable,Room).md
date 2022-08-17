# ShapeDrawable
안드로이드에서 UI를 구성할 때 외부 리소스를 쓸 수도 있지만 직접 drawable관련 xml 파일을 만들어서 리소스로 사용할 수 있다.   

## > 컴파일된 리소스 데이터 유형
GradientDrawable에 대한 리소스 포인터이다.   

## > 요소
1. `<shape>`   
shapeDrawable의 루트 요소이다.

    ```html
    <shape
        xmlns:android="http://schemas.android.com/apk/res/android"
        /// 사각형
        android:shape="rectangle"
        /// 타원형
        android:shape="oval"
        /// 가로선, stroke를 통해 너비 정의 필요    
        android:shape="line"
        /// 고리형
        android:shape="ring"
        >

        <corners ... />
        <gradient ... />
        <padding ... />
        <size ... />
        <solid ... />
        <stroke ... />

    </shape>
    ```

    만약 고리형일 경우   
        
    - `innerRadius`
    - `innerRadiusRatio`
    - `thickness`
    - `thicknessRatio`
    - `useLevel`

    등의 특성을 추가로 사용할 수 있다.

2. `<corners>`   
shape가 사각형일 경우에 사용한다. shape에 둥근 모서리를 생성한다.
    ```html
    <corners
        android:radius="integer"
        android:topLeftRadius="integer"
        android:topRightRadius="integer"
        android:bottomLeftRadius="integer"
        android:bottomRightRadius="integer" />
    ```

3. `<gradient>`   
shape에 대한 그라데이션 색상을 지정한다.

    ```html
    <gradient
        /// 그라데이션 각도 45의 배수로 설정
        android:angle="integer"
        /// 중심에 대한 상대적 X위치
        android:centerX="float"
        /// 중심에 대한 상대적 Y위치
        android:centerY="float"
        /// 시작 색상
        android:startColor="color"
        // 중간 색상
        android:centerColor="integer"
        /// 끝 색상
        android:endColor="color"
        /// 그라데이션 반경
        android:gradientRadius="integer"
        /// 그라데이션 패턴의 유형(선형)
        android:type="linear"
        /// 그라데이션 패턴의 유형(원형)
        android:type="radial"
        /// 그라데이션 패턴의 유형(스위핑 라인)
        android:type="sweep"
        /// LevelListDrawable로 사용되는 경우 true
        android:useLevel=["true" | "false"] />
    ```

4. `<padding>`

    ```html
    <padding
        android:left="integer"
        android:top="integer"
        android:right="integer"
        android:bottom="integer" />
    ```

5. `<size>`   
shape의 크기를 설정한다.
    ```html
    <size
        android:width="integer"
        android:height="integer" />
    ```

6. `<solid>`   
shape를 채울 단색을 설정한다.
    ```html
    <solid
        android:color="color" />
    ```

7. `<stroke>`   
shape에 대한 테두리 선을 설정한다.
    ```html
    <stroke
        android:width="integer"
        android:color="color"
        /// 점선 사이의 거리
        android:dashWidth="integer"
        /// 각 점선의 크기
        android:dashGap="integer" />
    ```

# Room Database
안드로이드에서 기기 내부에 데이터를 저장하는 방법은 크게 3가지가 있다. 먼저, 저번 프로젝트[(관련 TIL)](https://github.com/SeonJK/TIL/blob/main/Kotlin/220328(SharedPreference%2CAlertDialog%2CHandler).md)에서 사용한 SharedPreferences와 오늘 공부한 SQLite를 사용하는 Room Database, 그리고 Realm이 있다.(DataStore도 있는데 이건 뭔지 아직 모르겠다.) 보통 저장하려는 데이터의 크기에 따라서 SharedPreferences < Room < Realm을 골라서 사용한다고 한다.   
<br/>
오늘 공부한 Room Database는 SQLite를 활용한 DB이다. SQLite보다 Room을 사용하는 이유는 다음과 같다.

* SQLite의 문제점
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb62K8O%2Fbtq1MqJJ8C4%2FX75kx2TNsjTz7Gc7KV8mI0%2Fimg.png" width="90%" height="70%"></img>

* Room의 장점
1. SQL 쿼리의 컴파일 시간 확인
2. 반복적이고 오류가 발생하기 쉬운 상용구 코드를 최소화하는 편의 주석
3. 간소화된 데이터베이스 이전 경로

## > 기본 구성요소
* Database class: 디비를 보유하고 앱의 영구 데이터와의 기본 연결을 위한 기본 액세스 포인트 역할을 한다.
* Entity: 앱 디비의 테이블을 나타낸다.
* DAO(Data Access Object): 앱이 디비의 데이터를 쿼리, 수정, 삽입, 삭제하는 데 사용할 수 있는 메소드를 제공한다.
<img width="80%" height="80%" src="https://developer.android.com/images/training/data-storage/room_architecture.png?hl=ko"></img>

## > 사용법
1. build.gradle   
Room을 사용하기 위해 dependencies 작업을 해주어야 한다.

    ```groovy
    dependencies {
        implementation "androidx.room:room-runtime:$room_version"
        annotationProcessor "androidx.room:room-compiler:$room_version"
    }
    ```

2. Entity   
Entity란 디비에서 한 테이블의 정보를 말하는 정보 단위이다. 디비에 테이블을 만드는 작업과 같다.   
<br/>
먼저 java 패키지 폴더 안에 새로운 model 패키지 폴더를 생성한다. model 패키지 안에 원하는 테이블의 이름을 적은 클래스 파일을 만든다.

    ```kotlin
    // 계산기 히스토리 테이블

    @Entity
    data class History(
        // Column 생성
        @ColumnInfo(name="expression") val expression: String?,
        @ColumnInfo(name="result") val result: String?
    ) {
        // 생성자 인자에서도 기본키를 설정할 수 있지만
        // autoGenerate 속성을 주기 위해 멤버 변수로 설정
        @PrimaryKey(autoGenerate = true) var uid: Int = 0
    }
    ```

3. DAO   
DAO란 Data Access Object의 약자로 디비에 접근할 수 있는 메소드를 정의하는 인터페이스를 말한다.   
<br/>
DAO 파일도 자바 패키지 폴더 안에 dao 패키지 폴더를 생성한다. dao 패키지 폴더 안에 클래스 파일을 생성하여 작업한다.

    ```kotlin
    @Dao
    interface HistoryDao {
        @Insert
        fun insert(user: User)

        @Update
        fun update(user: User)

        @Delete
        fun delete(user: User)
    }
    ```

    삽입/수정/삭제는 기본 메소드로 어노테이션만 붙이면 바로 사용할 수 있다. 이외에 다른 기능을 사용하려면 어떻게 해야 할까?   
    다른 기능을 하는 메소드를 사용하고 싶다면 직접 쿼리를 작성하여 만들 수 있다.
    
    ```kotlin
    @Dao
    interface HistoryDao {
        @Query("SELECT * FROM History")
        fun getAll(): List<History>

        @Query("DELETE FROM History")
        fun deleteAll()
    }
    ```

   SQL 쿼리는 임용 준비하면서 공부했기에 기본은 작성할 수 있어서 따로 설명은 생략!

4. Room Database   
이제 디비를 생성하고 관리하는 데이터베이스 객체를 만들어야 한다. 이 때는 추상 클래스를 사용한다.

    ```kotlin
    // entities에는 2.에서 만든 entity를 넣어주면 된다.
    // version은 entity의 구조를 변경해야 할 경우 이전 구조와 현재 구조를 구분할 수 있는 역할을 수행한다.
    // 구조가 바뀌었는데 버전이 같으면 에러가 뜬다.
    @Database(entities = [History::class], version = 1)
    abstract class AppDatabase : RoomDatabase() {
        abstract fun historyDao(): HistoryDao
    }

    // entity가 두개 이상을 경우 arrayOf() 안에 콤마로 구분하여 넣어주면 된다.
    @Database(entities = arrayOf(History::class, User::class), version = 1)
    abstract class AppDatabase : RoomDatabase() {
        abstract fun historyDao(): HistoryDao
    }
    ```

    공식문서에서는 디비 인스턴스를 만들 때 싱글톤으로 구현하는 것을 권장하고 있다. 싱글톤으로 만들면 신경쓸 부분이 많지만 객체 생성 비용을 줄일 수 있다.
    
    ```kotlin
    @Database(entities = arrayOf(History::class), version = 1)
    abstract class AppDatabase : RoomDatabase() {
        abstract fun historyDao(): HistoryDao

        companion object {
            private var INSTANCE: AppDatabase? = null

            @Volatile
            fun getInstance(context: Context): AppDatabase? {
                return INSTANCE ?: synchronized(this){
                    val instance = Room.DatabaseBuilder(
                        // 컨텍스트
                        context.applicationContext,
                        // 디비 객체 파일
                        AppDatabase::class.java,
                        // 디비 이름 정의
                        "history-database"
                    ).build()
                    INSTANCE = instance
                    // instance 리턴
                    instance
                }
            }
        }
    }    
    ```
    
    코드를 살펴보면 `@Volatile`을 볼 수 있다.
    이는 Java 변수를 Main Memory에 저장하겠다는 것을 명시하는 어노테이션이다. 변수를 Read할 때마다 CPU Cache가 아닌 Main Memory에서 읽어오겠다는 뜻이다.    
    <br/>
    CPU Cache가 훨씬 속도가 빠른데 왜 이걸 사용할까?   
    <br/>
    멀티스레드 환경에서 변수 값을 CPU Cache에 저장하게 되면 각각 저장된 값이 달려지는 불일치 문제가 생길 수 있기 때문이다. [(예제)](https://jw910911.tistory.com/56#:~:text=%EC%83%9D%EA%B8%B8%20%EC%88%98%20%EC%9E%88%EC%8A%B5%EB%8B%88%EB%8B%A4.-,%EC%98%88%EC%A0%9C\)%C2%A0,-%EC%98%88%EB%A5%BC%20%EB%93%A4%EC%96%B4%20SharedObject)   
    <br/>
    Volatile은 언제 적합할까?   
    <br/>
    멀티스레드 환경에서 하나의 스레드만 Read&Write하고 나머지 스레드는 Read하는 상황에 적합하다.   
    만약 다른 스레드도 Read&Write하는 상황에서는 Synchronized를 사용하여 변수의 원자성을 보장해야 한다.   
    <br/>
    그래서 코드에서는 전체적인 디비 인스턴스를 get하는 메소드는 Volatile로 구현하고, 생성할 때는 Synchronized로 구현하였다. 

5. 디비 인스턴스 생성   
인스턴스 생성은 클래스 파일에서 수행한다. lazy하게 선언해도 된다.

    ```kotlin
    // 싱글톤 패턴을 사용하지 않은 경우
    val db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "user-database"
    ).build()
    db.historyDao().insert(History(null, expressionText, resultText))

    // 싱글톤 패턴을 사용한 경우
    val database = WordRoomDatabase.getDatabase(this)
    db!!.historyDao().insert(History(null, expressionText, resultText))
    ```

6. 디비 사용   
디비 쿼리 작업은 긴 작업을 수행해야하기 때문에 메인 스레드인 UI 스레드 대신 새로운 작업 스레드를 만들어 작업하는 것이 좋다. 이걸 비동기 작업이라고 하는데 비동기 작업을 하는 방법에는 여러가지가 있다. 프로젝트에 사용하는 Thread, 요즘 핫한 것 같은 Coroutines, 그리고 Async Tasks 등.   
<br/>
이 중에서 Thread에 대해 간단하게 정리해보겠다.   

* Thread   
Android에서는 기본적으로 메인 스레드로 UI스레드를 생성한다. UI스레드에서는 UI 컴포넌트에 대한 시스템 호출(Touch, Swipe Event 등)을 수행한다.   
<br/>
이외에 리소스를 많이 소모하는 작업을 UI스레드가 모두 수행할 경우 앱은 낮은 성능을 보일 수 있다. 특히 긴 작업을 수행할 때 전체 UI가 차단되게 되고 모든 이벤트는 발송되지 않는다. 그래서 사용자에게는 앱이 중단된 것처럼 보이며 5초 이상 UI스레드가 차단되면 "어플리케이션이 응답하지 않습니다."라는 경고창이 뜨며 앱이 종료된다.   
<br/>
또, UI toolkit은 스레드로부터 안전하지 않다. 그래서 작업 스레드에서는 UI를 조작하지 않도록 하고, UI 작업은 UI스레드에서 해야한다.

    ```kotlin
    // db에 저장하는 스레드, UI작업 필요X
    Thread(Runnable {
        // 싱글톤 패턴을 사용하지 않은 경우
        db.historyDao().insert(History(null, expressionText, resultText))
        // 싱글톤 패턴을 사용한 경우
        db!!.historyDao().insert(History(null, expressionText, resultText))
    }).start()


    // db의 데이터를 가져오는 스레드, UI작업 필요O
    Thread(Runnable {
        db.historyDao().getAll().reversed().forEach {
            // UI작업은 UI스레드에게
            runOnUiThread {
                // ...
            }
        }
    })
    ```
