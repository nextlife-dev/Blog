---
title: concurrent.futures 모듈 사용
description: 
aliases:
  - Seungwoo Ham
tags:
  - high-level-programming
date: 2025-01-16
---
## 고급 동시성 처리 기능

```python
with concurrent.futures.ThreadPoolExecutor(max_workers=num_threads) as executor:
    futures = [executor.submit(download_image, url, folder) for _ in range(num_threads)]
```

스레드 풀을 만들고 관리하는 복잡한 로직을 추상화합니다.

이 모듈의 핵심은 `Executor` 클래스입니다. 두 가지 주요 구현체를 가지고 있죠:

- `ThreadPoolExecutor` 스레드 기반의 병렬 처리를 위한 클래스
- `ProcessPoolExecutor` 프로세스 기반의 병렬 처리를 위한 클래스

---

## Future 객체

비동기적으로 실행되는 작업의 결과를 나타냅니다.  
이 객체를 통해 작업의 상태를 확인하고 결과를 얻을 수 있습니다.

### 스레드 기반의 병렬 처리

우선 작업 제출하는 예제를 보죠:

```python
with concurrent.futures.ThreadPoolExecutor(max_workers=3) as executor:
    future = executor.submit(some_function, arg1, arg2)

```

여기서 `with` 구문은 [[2025/Resource/Python/Context Manager|Context Manager]]에서 설명했습니다.  
`max_workers`는 병렬 처리할 스레드의 수입니다.

`some_function` 함수의 매개변수가 3개라면 `submit` 메서드가 그거에 맞춰 처리할 수 있습니다.  
이 메서드는 가변 인자(\*args)를 사용하여 구현되어있기 때문에 필요한 만큼의 인자를 전달할 수 있습니다.

만약 작업이 끝났다면, Future 객체의 `result()` 메서드를 사용하여 작업의 결과를 얻을 수 있습니다:

```python
result = future.result()
```

### 프로세스 기반의 병렬 처리

```python
with concurrent.futures.ProcessPoolExecutor() as executor:
    results = list(executor.map(some_function, iterable))
```

`map()` 메서드를 사용하여 여러 작업을 동시에 처리할 수 있습니다.

`map()` 함수와 유사한 인터페이스를 제공하여 사용이 쉽습니다.  
결과가 입력한 순서대로 반환이 되기 때문에 예측하기 쉽고요.

만약 완료된 작업을 순서대로 결과 얻으려면 아래와 같이 작성할 수 있습니다:

```python
for future in concurrent.futures.as_completed(futures):
    result = future.result()
    # 결과 처리
```

### `submit()`과 `map()`의 차이점

예제를 유심히 보면, 매개변수를 전달하는 방식이 다릅니다.

```python
def add(x, y):
    return x + y

with concurrent.futures.ThreadPoolExecutor() as executor:
    future = executor.submit(add, 1, 4)
```

두 번째부터는 함수에 전달할 인자들을 직접 나열합니다.  
각 호출마다 개별적으로 인자를 지정할 수 있어 더 유연하게 사용할 수 있죠.

```python
def add(x, y):
    return x + y

with concurrent.futures.ThreadPoolExecutor() as executor:
    results = executor.map(add, [1, 2, 3], [4, 5, 6])
```

함수에 전달할 인자들의 이터러블을 순서대로 나열합니다.  
각 이터러블의 요소들이 함수의 인자로 순서대로 매핑됩니다.

따라서 `map()`은 동일한 함수를 여러 인자 세트에 적용할 때 편리하고,  
`submit()`은 각 작업마다 다른 인자를 전달해야 할 때 유용합니다.