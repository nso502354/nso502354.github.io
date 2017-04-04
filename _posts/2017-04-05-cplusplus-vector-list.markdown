---
layout: post
title:  "C++ - Vector, List"
date:   2017-04-05 00:30:00 +0900
categories: jekyll update
---

### vector (가변 배열)
  * 벡터는 크기가 변할 수 있는 배열을 나타내는 시퀀스 컨테이너
  * 연속적인 메모리(메모리를 연속적으로 할당)
  * 배열과 마찬가지로 벡터도 요소에 연속된 저장위치를 사용
    * 요소에 대한 일반 포인터의 오프셋을 사용하여 배열에서와 마찬가지로 요소에 액세스 가능
    * 배열과 달리 컨테이너의 크기는 동적으로 변경 될 수 있으며, 컨테이너가 컨테이너를 자동으로 처리합니다.
  * 내부적으로 벡터는 동적으로 할당 된 배열을 사용하여 요소를 저장합니다.
    * 이 배열은 새로운 요소가 삽입 될 때 크기가 커지기 위해 재할당되어야 할 수도 있습니다.
    * 새로운 배열을 할당하고 모든 요소를 이동한다는 의미입니다. 그러나 상대적으로 많은 비용이 드는 작업이므로 요소가 컨테이너에 추가 될 때마다 벡터가 재할당되지는 않습니다.
  * 어레이에 비해 벡터는 스토리지를 관리하고 효율적, 동적으로 확장할 수 있으나 더 많은 메모리를 사용
  * dynamic sequence containers(deques, lists and forward_lists)와 비교할 때 `벡터는 배열과 마찬가지로 요소에 액세스하는 것이 효율적`
    * <b>끝에서 요소를 추가하거나 제거하는 것이 효율적(상수 시간)</b>
    * <b>끝 이외의 위치에 요소를 삽입하거나 제거하는 작업의 경우에는 상수 시간이 아니다.(선형 시간)</b>

### list (리스트)
  * list는 시퀀스 내의 임의의 위치에서 일정 시간의 삽입 및 삭제 작업을 허용하고 양방향에서 반복을 허용하는 시퀀스 컨테이너
  * 비연속적인 메모리(메모리를 따로따로 할당)
  * list는 내부적으로 이중 연결 목록(doubly_linked lists)으로 구현되어 있습니다.
    * single-linked lists으로 구현되어 있는 forward_list와 흡사
  * 다른 기본 표준 시퀀스 (array, vector and deque)와 비교할 때, `list는 삽입 추출 및 이동하는 데 일반적으로 더 효과적`이다.
  * 다른 시퀀스 컨테이너와 비교할 때, list 및 forward_lists의 주요 단점은 요소에 직접 액세스 할 수 없다는 점입니다.
    * 예를 들어, 목록의 여섯 번째 요소에 액세스하려면 시작 위치 또는 끝 위치와 같은 알려진 위치에서 해당 위치까지 반복해야 합니다.(선형 시간)

```
    중간에 데이터 삽입, 삭제의 빈도가 적고 랜덤 접근을 자주 해야 한다면 vector
    중간에 데이터 삽입, 삭제가 빈번하며 랜덤 접근이 필요 없으면 list
```



> [C++ vector Reference][vector-Reference] / [C++ list Reference][list-Reference]

[vector-Reference]: http://www.cplusplus.com/reference/vector/vector/
[list-Reference]: http://www.cplusplus.com/reference/list/list/
