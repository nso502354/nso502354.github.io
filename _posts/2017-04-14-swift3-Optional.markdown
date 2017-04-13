---
layout: post
title:  "Swift - 옵셔널"
date:   2017-04-14 00:10:00 +0900
categories: jekyll update
---

## 옵셔널(Optional)

Apple Developer에서 Swift의 특징 중 `안전을 고려한 설계(Designed for Safety)`라고 설명한 것을 찾아볼 수 있습니다. 옵셔널로 표현된 변수/상수에 nil 혹은 적절한 값을 할당할 수 있습니다. (일반적인 변수/상수에는 nil을 할당할 수 없습니다.)

변수/상수에 옵셔널을 표현해주면 '해당 변수/상수에는 값이 없을 수 있다.', '변수/상수가 nil일 수도 있으므로 사용에 주의하라'는 뜻으로 보면 됩니다. iOS프로그래머가 Swift로 작성된 코드를 볼 때 옵셔널을 통해 어떠한 코드 블럭 부분에 포함된 프로퍼티, 메서드, 서브스크립트가 nil일 수 있는지 한눈에 알아볼 수가 있게 됩니다. 즉, 안전성뿐만 아니라 가독성 향상도 포함되는 것 같습니다.

또한, 이러한 옵셔널 처리를 Swift에서는 옵셔널 체이닝과 빠른 종료 구문으로 처리할 수 있습니다. 옵셔널 체이닝과 빠른 종료 구문은 다음 포스트에서 다루겠습니다.

> nil은 Objective-C에서의 NIL(레퍼런스 타입의 값이 없는 것)과 다릅니다. Swift에서는 레퍼런스 타입의 변수/상수 뿐만이 아니라 밸류 타입의 변/상수의 값이 없는 것을 의미합니다.  


### 옵셔널 개요

Swift는 컴파일러는 Compile-time error가 있는 객체를 만들거나 사용하지 못하게 합니다. 즉, 코드를 깔끔하고 안전하게 작성할 수 있게 하여 런타임 충돌을 방지할 수 있습니다. 예를 들어, 옵셔널이 아닌 변수에 nil을 설정하려하면 컴파일러는 nil값을 할당할 수 없다고 컴파일 타임에 오류를 발생시킵니다.

{% highlight swift %}

var message: String = "Swift is awesome!" // OK
message = nil // complie-time error

{% endhighlight %}


옵셔널은 "?" 오퍼레이터를 타입 선언 뒤에 추가하여 message라는 변수를 옵셔널로 표현해보면

{% highlight swift %}

var message: String? = "Swift is awesome!" // OK
message = nil // OK

{% endhighlight %}

또한, 옵셔널은 제네릭이 적용된 열거형임을 확인할 수 있습니다.

{% highlight swift %}

public enum Optional<Wrapped> : ExpressibleByNilLiteral {
    case none
    case some(Wrapped)

    public init(_ some: Wrapped)
    // ...
}

{% endhighlight %}


### 옵셔널 강제 추출(Forced Unwrapping)

  * 옵셔널 강제 추출은 옵셔널의 값을 추출하는 가장 간단한 방법이자 위험한 방법입니다.
  * 옵셔널 강제 추출은 런타임의 오류를 내포하고 있기 때문에 지양해야합니다.

  {% highlight swift %}

  var name: String? = "namsang"
  var namsang: String = name!

  name = nil
  namsang = name! // 런타임 오류!

  {% endhighlight %}


### 옵셔널 바인딩(Optional Binding)

  * 옵셔널 바인딩은 'if let / if var' 을 통해 nil인지 아닌지 확인하는 방식입니다.
  * 쉼표(,)를 사용하여 여러 옵셔널의 값을 추출할 수 있습니다.
    * 단, 바인딩하려는 옵셔널 중 하나라도 값이 없다면 블록 내부의 명령문은 실행되지 않습니다.

  {% highlight swift %}

  var name: String? = "namsang"

  if let unWrappedName = name {
      print("My name is \(unWrappedName)")
  } else {
      print("myName == nil")
  }


  {% endhighlight %}

## 참고

> 글의 일부 내용은 야곰님의 저서 [스위프트 프로그래밍][Swift-programming](2017, 한빛미디어)를 참고하여 작성되었습니다.

[Swift-programming]: http://book.naver.com/bookdb/book_detail.nhn?bid=11445773
