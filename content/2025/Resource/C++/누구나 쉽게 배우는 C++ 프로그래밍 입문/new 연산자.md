---
title: new 연산자
description: 
aliases: ["Seungwoo Ham"] 
tags: []
date: 2025-01-28
---
참고: [C++ Learn: new 연산자](https://learn.microsoft.com/ko-kr/cpp/cpp/new-operator-cpp?view=msvc-170)

Java:

```java
Integer num = new Integer(5); // 또는 그냥 Integer num = 5;
```

C++:

```cpp
int* pointer = new int;  // 힙 메모리에 정수형 공간 할당
*pointer = 5;  // 값 할당
```

Java는 가비지 컬렉터가 자동으로 메모리를 관리하지만, C++에서는 `delete pointer;`로 직접 메모리를 해제해줘야 합니다.

Java에서는 기본 타입(primitive type)을 제외한 모든 것이 참조(reference)인 반면, C++는 스택에 직접 객체를 만들 수도 있고 힙에 동적으로 할당할 수도 있습니다.

Java의 참조는 포인터와 비슷하지만, 직접적인 메모리 조작은 불가능합니다. C++의 포인터는 메모리 주소를 직접 다룰 수 있죠.

그래서 개념적으로는 비슷하다고 볼 수 있지만, 실제 동작 방식과 메모리 관리 측면에서는 꽤 다릅니다.