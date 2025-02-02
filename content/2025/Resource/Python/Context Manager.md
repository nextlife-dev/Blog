---
title: Context Manager
description: 
aliases:
  - Seungwoo Ham
tags:
  - high-level-programming
date: 2025-01-16
---
## `with` 구문

```python
with concurrent.futures.ThreadPoolExecutor(max_workers=num_threads) as executor:
```

Context Manager는 리소스의 획득과 해제를 자동으로 관리하는 고급 문법입니다.

`__enter__`와 `__exit__` 메서드가 자동으로 호출되어 리소스 관리를 안전하게 합니다.

→ https://docs.python.org/ko/3/library/contextlib.html
→ https://kimjingo.tistory.com/211

위의 주소를 통해 원리를 파악할 수 있습니다.