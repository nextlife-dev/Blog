---
title: WebSocket 통신 RestController 대신 Controller 사용해야 되는 이유
description: 
aliases:
  - Seungwoo Ham
tags:
  - spring
date: 2025-01-12
---
HTTP 기반의 핸드셰이크를 통해 연결을 설정한 후, 양방향 실시간 통신을 위한 별도의 프로토콜을 사용합니다. 즉 HTTP가 아닌 WebSocket 프로토콜 사용한다고 이해하면 되겠습니다.

이때 Spring Boot에서는 `@RestController`가 아닌 `@Controller`를 써야 하는 이유에 대해 조사해봤습니다.

## 특징 차이

`@RestController`는 REST 통신에서 필요한 `@ResponseBody` 포함합니다. 그래서 자동으로 JSON/XML Body 타입으로 바꿔서 응답해주는 방식으로 되어있습니다.

→ [Annotaion Interface RestController](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html)

그래서 이는 REST API의 Endpoints 만들 때 주로 사용합니다.

하지만 WebSocket 특성상 실시간 통신으로 이루어야 되기 때문에 반드시 반환되는 `@RestController`와는 맞지 않습니다. 그래서 `@Controller`를 써야 되는 이유가 바로 있습니다:

- 유연한 설정
	- WebSocket 관련 설정을 유연하게 가능합니다.
	- 예를 들어, 특정 Endpoints에 대한 핸들러 매핑이나 인터셉터 추가 등이 가능합니다.
- 전용 어노테이션
	- WebSocket 구현하기 위해 `@MessageMapping`과 같은 전용 어노테이션을 사용합니다.
- 단순한 정보 통신
	- WebSocket은 단순 메시지 교환이므로 기본 `@Controller`로 충분합니다.

## 예제

```java
@MessageMapping("/chat.send")
@SendTo("/topic/public")
```

바로 이 전용 어노테이션들이 `@Controller`와 함께 사용될 때 ==자연스럽게 동작== 합니다. `@RestController`의 자동 응답 변환 기능이 오히려 이러한 WebSocket 메시징 흐름을 방해할 수 있습니다.