---
layout: post
title:  "Java - Vector, List"
date:   2017-04-05 01:30:00 +0900
categories: jekyll update
---

Java에서 Vector와 ArrayList(List) 콜렉션은 내부적으로 배열로 구현되어 있으며 인덱스를 이용해 데이터 액세스가 가능합니다. Vector와 List의 차이점은 동기화(Synchronize)의 처리입니다. Vector 클래스는 자동으로 동기화를 보장해주기 때문에 멀티 스레드 환경이 아니면 ArrayList를 사용하는게 좋습니다.

> Vector는 자동으로 동기화 보장, ArrayList는 자동으로 동기화를 보장하지 않음

### Vector
  * Vector Class는 확장 가능한 객체 배열을 구현
  * 배열과 마찬가지로 정수 인덱스를 사용하여 액세스 할 수 있는 구성 요소 포함
  * Vector를 생성한 후에도 항목을 추가하거나 제거 할 수 있도록 필요에 따라 Vector의 크기 조절 가능
  * 각 벡터는 capacity 와 capacityIncrement를 유지하여 스토리지 관리를 최적하하려고 합니다.
  * `Vector는 자동으로 동기화를 보장(무조건 동기화)`됩니다. 복수의 스레드로부터 추가/삭제가 이루어지더라도 내부의 데이터 처리는 안전하게 한번에 하나의 스레드만 처리되도록 보장함으로써 데이터 처리의 안정화의 이점이 있습니다.
    * 단일 스레드 처리시에는 자동 동기화 보장이 오히려 성능의 저하를 일으킬 수 있기에, 단일 처리 상에는 ArrayList가 더 효율적인 성능을 보장.

### ArrayList(List)
  * ArrayList는 List 인터페이스의 Resizable-array의 구현
  * 모든 선택적 목록 작업을 구현하고 null을 포함하여 모든 요소를 허용
  * ArrayList는 비동기성을 제외하면 Vector와 거의 같습니다. 즉, 자동으로 동기화를 보장하지 않습니다.
    * 복수의 thread가 동시에 ArrayList 인스턴스에 액세스하는 경우에는 외부적으로 synchronized할 필요가 있습니다.


#### ArrayList 와 List?
List 는 배열에 기능을 추가한거고, ArrayList 는 List에 기능을 추가 한 것입니다. 이미 크기나 데이터가 정해져 있는 경우에, List배열을 쓰는게 속도 측면에서나 typeCasting의 번거로움이 없기에 좋습니다.
> List는 type을 지정할 수 있어서 box, unbox이 일어나지 않아 ArrayList보다 빠릅니다. 아이템 타입이 object인 ArrayList는 Reference타입이 아닌 value타입을 사용 시에 box, unbox에서 문제가 있습니다.




### 참고
> [Java Vector Reference][Vector-Reference] / [Java List Reference][List-Reference]

[Vector-Reference]: https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html
[List-Reference]: https://docs.oracle.com/javase/8/docs/api/java/util/List.html
