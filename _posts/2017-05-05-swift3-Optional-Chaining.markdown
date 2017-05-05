---
layout: post
title:  "Swift - 옵셔널 체이닝과 빠른 종료"
date:   2017-05-05 12:00:00 +0900
categories: jekyll update
---

## 옵셔널 체이닝(Optional Chaining)

옵셔널 체인은 현재 nil일지도 모르는 옵셔널에 속해 있는 프로퍼티, 메서드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정입니다.

옵셔널이 값을 가지고 있다면 프로퍼티, 메서드, 서브스크립트 등을 호출할 수 있고, 옵셔널이 nil이라면 프로퍼티, 메서드, 서브스클비트 등은 nil을 반환합니다.

옵셔널을 반복 사용하여 옵셔널이 자전거 체인처럼 서로 꼬리를 물고 있는 모양이기 때문에 옵셔널 체이닝이라고 부릅니다.

옵셔널 체이닝은 프로퍼티, 메서드 또는 서브스크립트를 호출하고 싶은 옵셔널 변수나 상수 뒤에 물음표(?)를 붙여서 표현합니다. 중첩된 옵셔널 중 하나라도 값이 존재하지 않는다면 결과적으로 nil을 반환합니다.  

```
Optional chaining in Swift is similar to messaging nil in Objective-C, but in a way that works for any type, and that can be checked for success or failure.

Swift에서의 옵셔널 체이닝은 Objective-C에서의 메시징 nil과 비슷하지만 모든 유형에 대해서 작동하고 성공 또는 실패 여부를 확인할 수 있는 방식으로 작동합니다.

```

#### 옵셔널 체이닝과 강제 추출

{% highlight swift %}

class Person {
    var residence: Residence?
}

class Residence {
    var numberOfRooms = 1
}

{% endhighlight %}

Residence 인스턴스는 한개의 Int 타입의 프로퍼티인 numberOfRooms를 가지고 있습니다. 그리고 Person 인스턴스는 옵셔널 형식의 residence 프로퍼티인 Residence?를 가지고 있습니다.

residence 프로퍼티는 옵셔널 형태(Residence?)이기 때문에 Person 인스턴스를 생성하려 할 때, residence 프로퍼티는 nil로 생성 됩니다.

{% highlight swift %}

let john: Person = Person()

{% endhighlight %}

느낌표(exclamation mark)를 통해서 강제 추출을 하여 john 인스턴스에서 residence 프로퍼티의 numberOfRooms에 접근해보겠습니다.

{% highlight swift %}

let roomCount: john.residence!.numberOfRooms
// this triggers a runtime error

{% endhighlight %}

residence 프로퍼티가 nil이기 때문에 강제 추출은 진행되지 않고 runtime error를 발생하게 됩니다. 하지만, `옵셔널 체이닝`을 이용한다면 numberOfRooms의 접근하는 일련의 과정을 성공과 실패로 구분 지을 수 있습니다.

{% highlight swift %}

if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// Prints "Unable to retrieve the number of rooms."

{% endhighlight %}


## 빠른 종료(Early Exit)

빠른 종료의 핵심 키워드는 guard입니다. guard 구문은 if 구문과 유사하게 Bool 타입의 값으로 동작하는 기능입니다. guard 뒤에 따라붙는 코드의 실행 결과가 true일 때 코드가 계속 실행됩니다. if 구문과는 다르게 guard 구문은 항상 else 구문이 뒤에 따라와야 합니다.

만약 guard 뒤에 따라오는 Bool 값이 false라면 else의 블록 내부 코드를 실행하게 되는데, 이때 else 구문의 블록 내부에는 꼭 자신보다 상위의 코드 블록을 종료하는 코드가 들어가게 됩니다. return, break, continue, throw 등의 제어문 전환 명령을 사용합니다. 또는 fatalError()와 같은 비반환 함수나 메서드를 호출할 수 있습니다.

#### guard 구문 사용 형식
{% highlight swift %}

guard Bool 타입 값 else {
    예외사항 실행문
    제어문 전환 명령어
}

{% endhighlight %}

Swift에서는 guard 구문을 사용하여 if 코드를 훨씬 간결하고 읽기 좋게 구성할 수 있습니다. if 구문을 쓰면 예외사항을 else 블록으로 처리해야 하지만 예외사항만을 처리하고 싶다면 guard 구문을 쓰는 것이 훨씬 간편합니다.


{% highlight swift %}

// if 구문을 사용한 코드
for i in 0...3 {
    if i == 2 {
        print(i)
    } else {
        continue
    }
}

// guard 구문을 사용한 코드
for i in 0...3 {
    guard i == 2 else {
      continue
    }
    print(i)
}
{% endhighlight %}


#### guard 구문으로 옵셔널 바인딩 사용

Bool 타입의 값으로 guard 구문을 동작시킬 수 있지만 옵셔널 바인딩의 역할도 수행할 수 있습니다. guard 뒤에 따라오는 옵셔널 바인딩 표현에서 옵셔널의 값이 있는 상태라면 guard 구문에서 옵셔널 바인딩된 상수를 guard 구문이 실행된 아래 코드부터 함수 내부의 지역상수처럼 사용할 수 있습니다.


{% highlight swift %}

func greet(_ person: [String: String]) {
    guard let name: String = person["name"] else {
        return
    }

    print("Hello \(name)")

    guard let location: String = person["location"] else {
        print("I Hope the weather is nice near you")
        return
    }

    print("I hope the weather is nice in \(location)")

}

var personInfo: [String: String] = [String: String]()
personInfo["name"] = "Jenny"

greet(personInfo)
// Hello Jenny
// I Hope the weather is nice near you

personInfo["location"] = "Korea"

greet(personInfo)
// Hello Jenny
// I Hope the weather is nice near in Korea

{% endhighlight %}

guard 구문을 통해 옵셔널 바인딩 된 상수는 greet(_ :) 함수 내에서 지역 상수로 사용된 것을 볼 수 있습니다.

> guard 구문은 if let과 마찬가지로 쉼표(,)로 추가 조건을 나열하여 옵셔널 바인딩이 가능합니다. `추가된 조건은 Bool 타입의 값이어야 합니다.` 또, 쉼표로 추가된 조건은 AND 논리연산과 같은 결과를 주기 때문에 쉼표를 &&로 치환해도 같은 결과를 얻을 수 있습니다.


## 참고

> 글의 일부 내용은 [Swift Language Guide][Swift-Closures]와 야곰님의 저서 [스위프트 프로그래밍][Swift-programming](2017, 한빛미디어)를 참고하여 작성되었습니다.

[Swift-programming]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html
[Swift-Closures]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html
