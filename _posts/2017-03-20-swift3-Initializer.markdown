---
layout: post
title:  "Swift - 인스턴스 생성 및 소멸"
date:   2017-03-20 19:30:00 +0900
categories: jekyll update
---

## 인스턴스 생성
* 저장 프로퍼티의 초깃값을 설정하는 일을 수행.
* 이니셜라이저를 정의하면 초기화 과정을 직접 구현 가능.
 > 이니셜라이저는 새로운 인스턴스를 생성할 수 있는 특별한 메서드, 인스턴스의 첫 사용을 위해 초기화하는 용도이며 반환 값은 없습니다.

```
class Student {
    init() {
        // 초기화 코드
    }
}

struct Lecture {
    init() {
        // 초기화 코드
    }
}

enum Major {
    case computerScience

    init() {
        // <b>열거형은 반드시 케이스 중 하나로 초기화 되어야 합니다.</b>
        self = .computerScience
    }
}
```


## 프로퍼티 기본값
* 구조체와 클래스의 인스턴스는 처음 생성될 때 옵셔널 저장 프로퍼티를 제외한 모든 저장 프로퍼티에 적절한 초깃값이 할당되어야 합니다.
* 초기화 후에 값이 확정되지 않은 저장 프로퍼티는 존재할 수 없습니다.


### 이니셜라이저로 저장 프로퍼티에 초깃값 설정
```
struct Volume {
    var size: Double

    init() {
        size =  0.0 // size의 초깃값 할당
    }
}

let volume: Volume = Volume()
print(volume.size) // 0.0
```

###  저장 프로퍼티에 초깃값을 설정
```
struct Volume {
    var size: Double = 0.0
}

let volume: Volume = Volume()
print(volume.size) // 0.0
```

> 이니셜라이저도 매개변수를 가질 수 있습니다. 인스턴스를 초기화하는 과정에서 필요한 값(argument, 인수)을 전달하여 초기화할 수 있습니다.

### 옵셔널 프로퍼티 타입
* 인스턴스가 사용되는 동안에 값을 꼭 가지지 않아도 되는 저장 프로퍼티가 있을 경우 사용.
* 초기화되는 과정에서 값을 지정해주기 어려운 경우, 저장 프로퍼티를 옵셔널로 선언합니다.

```
class Person {
    let name: String // 상수 프로퍼티
    var age: Int?

    init(name: String) {
        // <b>상수 프로퍼티는 초기화 과정에서만 값이 할당 될 수 있습니다.</b>
        self.name = name

    }
}
```

### 기본 이니셜라이즈, 멤버와이즈 이니셜라이저
* 기본 이니셜라이저는 프로퍼티의 기본값으로 프로퍼티를 초기화해서 인스턴스를 생성.
  * 저장 프로퍼티의 기본값이 모두 지정되어 있고, 동시에 사용자 정의 이니셜라이저가 정의되지 않아야합니다.
* 구조체는 사용자정의 이니셜라이저르 구현하지 않으면 프로퍼티의 이름으로 매개변수를 가지는 이니셜라이저인 <b>멤버와이즈 이니셜라이저</b>를 기본으로 제공.

### 초기화 위임
값 타입인 구조체와 열거형은 코드의 중복을 피하기 위하여 이니셜라이저가 다른 이니셜라이저에게 일부 초기화를 위임하는 초기화 위임을 간단하게 구현할 수 있습니다.
(초기화 위임은 최소 두 개 이상의 사용자 정의 이니셜라이저가 필요합니다.)
```
enum Student {
    case elemantary, middle, high
    case none

    init() {
        self = .none
    }

    init(KoreanAge: Int) {
        switch KoreanAge {
            case 8...13:
                self = .elemantary
            case 14...16:
                self = .middle
            case 17...19:
                self = .high
            default:
                self = .none
        }
    }

    init(bornAt: Int, currentYear: Int) {
        // self.init으로 사용자 정의 이니셜라이저로 초기화를 위임.
        self.init(KoreanAge: currentYear - bornAt + 1)
    }
}
```

### 실패 가능한 이니셜라이저(Failable initializer)
클래스, 구조체, 열거형에 모두 정의할 수 있으며 실패했을 때 nil을 반환합니다. `init 키워드 대신 init? 키워드를 사용합니다.`
> 실패 가능한 이니셜라이저가 특정 값을 반환하지는 않습니다. return nil(실패), return(성공)으로 성공과 실패를 표현할 뿐입니다.

```
enum Student {
    case elemantary, middle, high

    init?(KoreanAge: Int) {
        switch KoreanAge {
            case 8...13:
                self = .elemantary
            case 14...16:
                self = .middle
            case 17...19:
                self = .high
            default:
                // 초기화 실패를 표현
                return nil
        }
    }

    init?(bornAt: Int, currentYear: Int) {
        self.init(KoreanAge: currentYear - bornAt + 1)
    }
}
```

### 함수를 사용한 프로퍼티 기본값 할당
* 사용자정의 연산을 통해 저장 프로퍼티 기본값을 설정할 때 클로저나 함수를 사용.
> 클로저를 사용하여 프로퍼티 기본값을 설정할 때, 실행 시점은 이니셜라이즈(초기화)입니다. 따라서 다른 프로퍼티의 값이 세팅되기 전이기에 다른 프로퍼티를 사용하여 연산할 수 없습니다.

```
struct Student {
    var name: String?
    var number: Int?
}

class SchoolClass {
    var student: [Student] = {
          // 새로운 인스턴스를 생성하고 사용자정의 연산을 통한 후 반환해줍니다.
          // 반환되는 값의 타입은 [Student] 타입이어야 합니다.

          var arr: [Student] = [Student]()

          for num in 1...15 {
              var student: Student = Student(nam: nil, number: num)
              arr.append(student)
          }

          return arr
      }()
}

```

### 인스턴스 소멸
* 클래스의 인스턴스는 디이니셜라이저(Deinitializer)를 구현할 수 있습니다. 디이니셜라이저는 인스턴스가 메모리에서 해제되기 직전 클래스 인스턴스와 관련하여 원하는 정리 작업을 구현할 수 있습니다.
* deinit 키워드를 사용하며, 클래스의 인스턴스가 메모리에서 소멸되기 바로 직전에 호출됩니다.
* 스위프트는 인스턴스가 더이상 필요하지 않으면 자동으로 메모리에서 소멸시킵니다.(ARC)
* 인스턴스 내부에서 외부 리소스를 사용했다면, 인스턴스가 소멸하기 직전에 파일을 다시 저장하고 닫아주는 등의 부가 작업이 필요한 경우에 유용하게 사용합니다.



> 글의 일부 내용은 야곰님의 저서 [스위프트 프로그래밍][Swift-programming](2017, 한빛미디어)를 참고하여 작성되었습니다.

[Swift-programming]: http://book.naver.com/bookdb/book_detail.nhn?bid=11445773
