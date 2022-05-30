# Data Class
* JAVA에서는 pojo class(특정한 수행없이 비어있는 틀과 같은 클래스)를 만드려면 직접 일일이 작성해야 했다. 그리고 class는 파일에 하나만 만들 수 있기 때문에 파일이 매우 많이 늘어나고 효율이 떨어졌었다.
* Kotlin에서는 data class를 활용하여 pojo class를 손쉽게 만들 수 있고, 한 파일에 여러 data class를 만들어서 관리할 수 있다.
```kotlin
data class Ticket(val companyName: String, val name: String, var date: String, var seatNumber: Int)

fun main(){
    val ticketA = Ticket("koreanAir", "JK Seon", "2022-03-02", 14)
    
    println(ticketA)    //Ticket(companyName=koreanAir, name=JK Seon, date=2022-03-02, seatNumber=14)
}
```

# Companion Object
Static 메소드나 변수를 선언할 때 사용한다. 또한 이를 사용하면 private properties를 읽어올 수 있다.

```kotlin
class Book private constructor(val id: Int, val name: String){
    
    companion object /*이름 설정 가능*/{
        val myBook = "new book"
        fun create() = Book(0, myBook)
    }
}

fun main(){
    val book = Book.Companion.create()
    //이름을 설정했을 경우 Companion 대신 설정한 이름을 넣어준다.
    println("${book.id}, ${book.name}") //0, new book
}
```

Interface를 implements하여 사용할 수도 있다.
```kotlin
interface IdProvider{
    fun getId(): Int
}

class Book private constructor(val id: Int, val name: String){
    
    companion object: IdProvider{
        val myBook = "new book"

        override fun getId(){
            return 88
        }
        fun create() = Book(getId(), myBook)
    }
}
```

# Object
Kotlin에서는 Object를 활용해서 쉽게 Singleton Pattern을 만들 수 있다.
```kotlin
data class Car(val horsePower: Int)

object CarFactory{
    val cars = mutableListOf<Car>()
    fun makeCar(horsePower: Int): Car{
        val car = Car(horsePower)
        cars.add(car)
        return car
    }
}

fun main(){
    val car = CarFactory.makeCar(10)
    val car2 = CarFactory.makeCar(200)

    println(car)                    //Car(horsePoewr=10)
    println(car2)                   //Car(horsePoewr=200)
    println(Carfactory.cars.size)   //2
}

```

# Motion Layout
Android Studio 4.0 버전부터 Motion Editor를 지원하게 되어 Device를 돌리지 않고도 Editor에서 Motion을 확인할 수 있게 되었다.
* Motion Editor가 IntelliJ에는 지원하지 않을까 Motion Layout 예제를 찾아서 해보았는데...
* 잘 된다!!

## 특징
* Constraint Layout의 Subclass이다. → Constraint Layout에서 Convert하여 사용한다.
* Constraint Layout과 Transition Manager를 합친 기능을 제공한다 .
* 애니메이션이나 Transition을 xml파일 안에 모두 서술할 수 있다.

# MotionLayout 예제를 다루면서
안드로이드 스튜디오를 개발할 때 컨텐츠 접근성에 신경쓰도록 개발하라고 하는 메시지를 띄운다.

> Missing 'contentDescription' attribute on image

컨텐츠 접근성이란 컨텐츠가 사용자에게 제대로 보여지지 않을 경우(Ex> 엑박) 컨텐츠에 대한 설명을 적어놓아서 컨텐츠가 보이지 않더라고 설명을 보고 알 수 있게끔 해놓는 조치를 말한다.
<br/><br/>

예제에서 사용한 ImvageView에서 컨텐츠 접근성을 주기 위해서는 두 가지 방법이 있다.

* 1. xml을 이용하는 방법
```xml
android:contentDescription="String 추가"
```
* 2. Class를 이용하는 방법
```java
imageView = view.findViewById(R.id.image_view);
imageView.setContentDescription(getResources().getString(R.string.content_description));
```

ImageView에는 꼭 컨텐츠 접근성이 필요한 것은 아니라고 한다. ListView 또는 TabLayout의 경우는 오히려 설명을 포함하면 탐색 속도가 늦춰지기 때문에 좋지 않다.
