---
title: WebSocket
description: 
aliases:
  - Seungwoo Ham
tags:
  - document
date: 2025-01-09
---
## 개념

**웹 브라우저와 서버 간의 양방향 실시간 통신**을 가능하게 하는 프로토콜입니다.  
HTTP와 달리 ==연결을 유지==하면서 양쪽이 자유롭게 데이터를 주고 받을 수 있게 하죠.

기존 HTTP 통신에 한계가 있습니다. 예를 들어:

- 연결을 유지하려면 주기적인 요청(폴링)이 필요합니다. 브라우저 자체에서 연결해뒀다가 오가는 것이 없으면 자동으로 끊어집니다.
- 매 요청마다 새로운 연결을 맺고 끊는 데에 비용이 들어갑니다.

만약 웹 소켓을 사용하면 위의 한계를 극복할 수 있습니다.

---

## 동작 방식

Handshake (연결 수립)을 통해 연결을 시작하고 끝맺음할 수 있는데, 이를 통해 연결을 유지할 수 있죠. 사실 클라이언트나 서버를 개발할 때 Handshake라는 개념이 필수적이죠

→ https://aws-hyoh.tistory.com/39

 또한, 이런 프로토콜을 사용할 수 있습니다: `ws://`, `wss://`

→ https://stackoverflow.com/questions/46557485/difference-between-ws-and-wss

## 구현

좋습니다! 이를 Spring Boot이랑 React으로 간단한 웹 사이트를 만들어보죠.

음.. 실시간 채팅이 좋겠습니다!

- Spring Boot
	- [[2025/Resource/Spring Boot/WebSocket 통신 RestController 대신 Controller 사용해야 되는 이유|WebSocket 통신 RestController 대신 Controller 사용해야 되는 이유]]