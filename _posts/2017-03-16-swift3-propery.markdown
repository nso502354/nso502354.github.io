---
layout: post
title:  "Swift - 프로퍼티의 종류"
date:   2017-03-16 23:53:00 +0900
categories: jekyll update
---
### 프로퍼티
프로퍼티는 클래스, 구조체 또는 열거형 등에 관련된 값을 뜻합니다.

1. 저장 프로퍼티(Stored Properties)
  * 인스턴스 변수 또는 상수를 의미합니다.
2. 연산 프로퍼티(Computed Properties)
  * 값을 저장한 것이 아니라 특정 연산을 수행한 결과값을 의미합니다.
3. 타입 프로퍼티(Type Properties)
  * 특정 타입에 사용되는 것



### 저장 프로퍼티
```
struct CoordinatePoint {
  var x: Int  //저장 프로퍼티
  var y: Int  //저장 프로퍼티
}

// 구조체는 기본적으로 저장 프로퍼티를 매개변수로 가지는 이니셜라이저가 있습니다.
// 저장 프로퍼티가 옵셔널이 아니더라도, 구조체는 저장 프로퍼티를 모두 포함하는 이니셜라이저를 자동으로 생성합니다.
let namsangPoint: CoordinatePoint = CoordinatePoint(x: 10, y: 5)

class Position {
  var point: CoordinatePoint // 저장 프로퍼티(변수) - 위치(Point)는 변경될 수 있음을 뜻합니다.
  let name: String // 저장 프로퍼티(상수)

  // 클래스의 저장 프로퍼티는 옵셔널이 아니라면 프로퍼티 기본값을 지정해주거나 사용자 정의 이니셜라이저를 통해 반드시 초기화해주어야 합니다.
  init(name: String, currentPoint: CoordinatePoint) {
    self.name = name
    self.point = currentPoint
  }
}

// 사용자정의 이니셜라이저를 호출해야만 합니다.
// 그렇지 않으면 프로퍼티 초기값을 할당할 수 없기 때문에 인스턴스 생성이 불가능합니다.
let namsangPosition: Position = Position(name: "namsang", currentPoint: namsangPoint)

```

클래스의 저장 프로퍼티에 초깃값을 지정해주면 따로 사용자 정의 이니셜라이저를 구현해줄 필요가 없습니다.


```
struct CoordinatePoint {
  var x: Int = 0 // 저장 프로퍼티
  var y: Int = 0 // 저장 프로퍼티
}

// 프로퍼티의 초깃값을 할당했다면 굳이 전달인자로 초깃값을 넘길 필요가 없습니다.
let namsangPoint: CoordinatePoint = CoordinatePoint()

class Position {
  var point: CoordinatePoint = CoordinatePoint() // 저장 프로퍼티
  var name: String = "Unknown" // 저장 프로퍼티
}

// 초깃값을 지정해줬다면 사용자정의 이니셜라이저를 사용하지 않아도 됩니다.
let namsangPosition: Position = Position()

// 인스턴스를 생성한 후에 값을 할당해줄 수 있습니다.
namsangPosition.point = namsangPoint
namsangPosition.name = "namsang"

```

### 옵셔널 저장 프로퍼티

```
struct CoordinatePoint {
  // 위치는 x, y 값을 모두 가져야하므로 옵셔널이면 안됩니다.
  var x: Int
  var y: Int
}

class Position {
  // 현재 사람의 위치를 모를 수도 있습니다. - 옵셔널
  var position: CoordinatePoint?
  let name: String

  init(name: String) {
    self.name = name
  }
}

// 이름은 필수지만 위치는 모를 수 있습니다.
let namsangPosition: Position = Position(name: "namsang")

// 위치를 알게되면 그 때 위치 값을 할당해줍니다.
namsangPosition.point = CoordinatePoint(x: 20, y: 10)


```

### 지연 저장 프로퍼티

인스턴스가 생성될 때 프로퍼티에 값이 필요 없을 경우에 옵셔널 저장 프로퍼티로 정의합니다. 그러나 호출이 있어야 값을 초기화하는 지연 저장 프로퍼티(Lazy Stored Properties)가 있습니다. 지연 저장 프로퍼티는 var 키워드로 정의된 변수에 lazy 키워드를 사용하여 정의합니다.

지연 저장 프로퍼티는 `주로 복잡한 크래스나 구조체를 구현할 때` 많이 사용되며 불필요한 성능저하나 공간 낭비를 줄일 수 있습니다.

```
struct CoordinatePoint {
  var x: Int = 0
  var y: Int = 0
}

class Position {
  lazy var point: CoordinatePoint = CoordinatePoint()
  let name: String

  init(name: String) {
    self.name = name
  }
}

let namsangPosition: Position = Position(name: "namsang")

// Position 클래스의 point 지연 저장 프로퍼티를 호출(접근)했을 때 프로퍼티의 CoordinatePoint가 생성됩니다.
print(namsangPosition.point)
```

### 연산 프로퍼티
연산 프로퍼티는 특정 상태에 따른 값을 연산하는 프로퍼티입니다. 접근자, 설정자의 역할을 수행할 수 있으며 클래스, 구조체, 열거형에 정의할 수 있습니다.
 - 메서드로 접근자, 설정자를 구현헀을 때보다 연산 프로퍼티로 구현했을 때 훨씬 더 간편하고 직관적입니다.

### 메서드로 구현된 접근자와 설정자
```
 struct CoordinatePoint {
   var x: Int // 저장 프로퍼티
   var y: Int // 저장 프로퍼티

   // 대칭점을 구하는 메서드 - 접근자
   func oppositePoint() -> CoordinatePoint {
     return CoordinatePoint(x: -x, y: -y)
   }

   // 대칭점을 설정하는 메서드 - 설정자
   mutating func setOppositePoint(_ opposite: CoordinatePoint) {
     x = -opposite.x
     y = -opposite.y
   }
 }
```
### 연산 프로퍼티의 정의

```
 struct CoordinatePoint {
   var x: Int // 저장 프로퍼티
   var y: Int // 저장 프로퍼티

   // 연산 프로퍼티
   var oppositePoint: CoordinatePoint {
     // 접근자
     get {
       return CoordinatePoint(x: -x, y: -y)
     }

     // 설정자
     set(opposite) {
       x = -opposite.x
       y = -opposite.y
     }
   }
```

### 프로퍼티 감시자

프로퍼티 감시자(Property Observers)를 사용하면 프로퍼티의 값이 변경됨에 따라 적절한 액션을 취할 수 있습니다. 프로퍼티 감시자는 프로퍼티의 값이 새로 할당될 때마다 호출되는데 변경되는 값이 현재의 값과 같더라도 호출됩니다.

프로퍼티 감시자는 일반 저장 프로퍼티에만 적용할 수 있습니다.

* willSet : 프로퍼티의 값이 변경되기 직전에 호출
* didSet : 프로퍼티의 값이 변경된 직후에 호출

willSet 메서드에 전달되는 전달인자는 변경될 값(newValue), didSet 메서드에 전달되는 전달인자는 변경되기 전의 값(oldValue)이 있습니다.

### 타입 프로퍼티
각각의 인스턴스가 아닌 타입 자체에 속하게 되는 프로퍼티를 <b>타입 프로퍼티</b>라고 합니다. 타입 프로퍼티는 타입 자체에 영향을 미치는 프로퍼티이며 인스턴스의 생성 여부와 상관 없이 타입 프로퍼티의 값은 하나입니다.

* 해당 타입의 모든 인스턴스가 공통으로 사용하는 값
* 모든 인스턴스에서 공용으로 접근하고 값을 변경할 수 있는 변수

### 타입 프로퍼티와 인스턴스 프로퍼티
```
class TypeClass {

  // 저장 타입 프로퍼티
  static var typeProperty: Int = 0

  // 저장 인스턴스 프로퍼티
  var instanceProperty: Int = 0 {
    didSet {
      Type.typeProperty = instanceProperty + 100
    }
  }

  // 연산 타입 프로퍼티
  static var typeComputedProperty: Int {
    get {
      return typeProperty
    }
    set {
      typeProperty = newValue
    }
  }
}

```





> 글의 일부 내용은 야곰님의 저서 [스위프트 프로그래밍][Swift-programming](2017, 한빛미디어)를 참고하여 작성되었습니다.

[Swift-programming]: http://book.naver.com/bookdb/book_detail.nhn?bid=11445773
