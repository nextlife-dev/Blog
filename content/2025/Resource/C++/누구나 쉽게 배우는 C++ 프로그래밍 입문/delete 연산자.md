---
title: delete 연산자
description: 
aliases: ["Seungwoo Ham"] 
tags: []
date: 2025-01-28
---
참고: [C++ Learn: delete 연산자](https://learn.microsoft.com/ko-kr/cpp/cpp/delete-operator-cpp?view=msvc-170)

사용한 메모리를 다시 메모리 pool로 환수하며, 환수된 메모리는 프로그램의 다른 부분이 다시 사용됩니다.

여기에 4가지 규칙이 있습니다:

1. `new`로 대입하지 않은 메모리는 `delete`로 해제할 수 없으며,
2. 같은 메모리 블록을 연달아 두 번 `delete`로 해제할 수 없으며,
3. `new[]`로 메모리를 대입할 경우 `delete[]`로 해제하며,
4. 대괄호를 사용하지 않았다면 `delete`도 대괄호를 사용하지 않아야 합니다.