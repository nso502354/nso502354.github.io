---
layout: post
title:  "Swift - 클로저"
date:   2017-04-07 15:30:00 +0900
categories: jekyll update
---

## 클로저(Closure)

클로저는 코드에서 전달되고 사용할 수 있는 기능의 self-contained block 입니다. Swift의 클로저는 C 및 Objective-C 의 블록 또는 다른 프로그래밍 언어의 Lambda와 유사합니다. 일정 기능을 하는 코드를 하나의 블록으로 모아 놓은 것을 말합니다.

클로저는 정의된 컨텍스트에서 모든 상수 및 변수에 대한 참조(Reference)를 획득(Capture)하고 저장할 수 있습니다. 이는 상수나 변수에 대해 closing이라고 합니다. Swift는 획득(Capture)를 위한 모든 메모리 관리를 스스로 합니다.

Swift는 함수형 프로그래밍 패러다임과 프로토콜 지향 프로그래밍 패러다임을 강조합니다. 함수를 일급 객체로 다루는 스위프트 언어는 클로저를 사용하여 프로그램을 구현할 수 있습니다.

클로저를 통해 비동기 콜백 형태로 코드를 작성할 수 있습니다. 이를 통해, 클로저는 비동기 작업에 많이 사용하게 됩니다. 즉, 클로저를 잘 숙지한다면 `함수형 프로그래밍 패러다임의 가장 큰 장점인 대규모 병렬처리가`  굉장히 쉬워진다는 점입니다.

#### 클로저의 세 가지 모양
  * 이름을 가지면서 어떤 값도 획득하지 않는 전역함수의 형태
  * 이름을 가지면서 다른 함수 내부의 값을 획득할 수 있는 중첩된 함수의 형태
  * 이름이 없고 주변 문맥에 따라 값을 획득할 수 있는 축약 문법으로 작성된 형태

#### Swift 클로저의 표현 방법들

> Swift's closure expressions have a clean, clear style, with optimizations that encourage brief, clutter-free syntax in common scenarios. These optimizations include

  * 클로저는 매개변수와 반환 값의 타입을 문맥을 통해 유추할 수 있기 때문에 매개변수와 반환 값의 타입을 생략할 수 있습니다.
    * Inferring parameter and return value types from context.
  * 클로저에 단 한 줄의 표현만 들어있다면 암시적으로 이를 반환 값으로 취급합니다.
    * Implicit returns from single-expression closures.
  * 축약된 전달인자 이름을 사용할 수 있습니다. ($ 표시)
    * Shorthand argument names.
  * 후행 클로저 문법을 사용할 수 있습니다. (CompletionHandler 등과 같은 관용 표현)
    * Trailing closure syntax

#### 클로저의 기본 형태


{% highlight swift %}
{ (parameters) -> return type in
  statements
}

{ (매개변수들) -> 반환타입 in
 실행코드
}
{% endhighlight %}



### 기본 클로저

{% highlight swift %}
// 기본 클로저
office.sorted(by: {(first: String, second: String) -> Bool in
    return first > second
})
{% endhighlight %}

`in -> 매개변수 및 반환 타입과 실행 코드를 구분하기 위해 사용하는 키워드`

### 후행 클로저

{% highlight swift %}
// 후행 클로저
office.sorted() {(first: String, second: String) -> Bool in
    return first > second
}
{% endhighlight %}

함수나 메서드의 마지막 전달인자로 위치하는 클로저는 함수나 메서드의 소괄호를 닫은 후 작성해도 됩니다. 단, 후행 클로저는 맨 마지막 전달인자로 전달되는 클로저에만 해당됩니다.

### 후행 클로저 소괄호 생략

{% highlight swift %}
// 후행 클로저, 소괄호 생략
office.sorted {(first: String, second: String) -> Bool in
    return first > second
}
{% endhighlight %}

단 하나의 클로저만 전달인자로 전달하는 경우에는 소괄호를 생략해줄 수 있습니다.

### 문맥을 통한 타입 유추

{% highlight swift %}
// 후행 클로저, 소괄호 생략, 문맥을 통한 타입 유추
office.sorted {(first, second) -> Bool in
    return first > second
}
{% endhighlight %}

`타입 생략이 가능한 이유는 전달인자로 전달될 클로저가 이미 그 타입을 준수하고 있기 때문입니다.`

### 단축 인자 이름
{% highlight swift %}
// 후행 클로저, 소괄호 생략, 단축 인자 이름
office.sorted {
    // 단축 인자 표현을 사용하게 되면
    // 매개 변수 및 반환 타입과 실행 코드를 구분지을 in 키워드는 불필요합니다.
    return $0 > $1
}
{% endhighlight %}

### 암시적 반환 표현
{% highlight swift %}
// 후행 클로저, 소괄호 생략, 암시적 반환 표현
office.sorted {
    $0 > $1
}
{% endhighlight %}

반환 값을 갖는 클로저이고 클로저 내부의 실행문이 단 한줄이라면, 암시적으로 그 실행문이 반환 값으로 사용됩니다.

### 탈출 클로저
탈출 클로저는 함수의 동작이 끝난 후 어떠한 작업의 필요성이 있을 때 사용합니다. @escaping 키워드를 사용하여 클로저가 탈출한다는 것을 명시해줄 수 있습니다.

{% highlight swift %}

let firstClosure: (() -> Void) = {
    print("Closure A")
}

let secondClosure: (() -> Void) = {
    print("Closure B")
}


// first와 second 매개변수 클로저는 함수의 반환 값으로 사용될 수 있으므로 탈출 클로저입니다.
func returnOneClosure(first: @escaping (() -> Void), second: @escaping (() -> Void), shouldReturnFirstClosure: Bool) -> (() -> Void) {

    // 전달인자로 전달받은 클로저가 함수 외부로 다시 반환되기 때문에 함수를 탈출하는 클로저입니다.
    return shouldReturnFirstClosure ? first : second
}

// 함수에서 반환된 클로저가 함수 외부의 상수에 저장되었습니다.(함수는 일급객체)
// 클로저는 참조 타입입니다. 이 것은 해당 클로저의 참조를 할당하는 것입니다.
let returnedClosure: (() -> Void) = returnOneClosure(first: firstClosure, second: secondClosure, shouldReturnFirstClosure: true)

returnedClosure() // Closure A
{% endhighlight %}


### 비탈출 클로저와 탈출 클로저
{% highlight swift %}

func functionWithNoEscapeClosure(closure: (() -> Void)) {
    closure()
}

func functionWithEscapingClosure(completionHandler: @escaping (() -> Void)) -> (() -> Void)  {
    return completionHandler
}


class SomeClass {
    var x = 10

    func runNoEscapeClosure() {
        functionWithNoEscapeClosure {
            // 비탈출 클로저에서는 해당 타입의 프로퍼티, 메서드, 서브스크립트 등에 접근할 때 self 키워드를 꼭 써주지 않아도 됩니다.
            x = 200
        }
    }

    func runEscapingClosure() -> (() -> Void) {
        return functionWithEscapingClosure {
            // 탈출 클로저에서는 명시적으로 self 키워드를 사용하여 해당 타입의 프로퍼티, 메서드, 서브스크립트 등에 접근해야 합니다.
            self.x = 100
        }
    }
}

let instance: SomeClass = SomeClass()
instance.runNoEscapeClosure()
print(instance.x)

let returnedClosure: (() -> Void) = instance.runEscapingClosure()
returnedClosure()
print(instance.x)
{% endhighlight %}


## 참고

> 글의 일부 내용은 [Swift Language Guide][Swift-Closures]와 야곰님의 저서 [스위프트 프로그래밍][Swift-programming](2017, 한빛미디어)를 참고하여 작성되었습니다.

[Swift-programming]: http://book.naver.com/bookdb/book_detail.nhn?bid=11445773
[Swift-Closures]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html
