---
title: Simply to pay, Best the cycle
description: 
aliases:
  - Seungwoo Ham
tags:
  - project
date: 2024-12-10
---
## 개요

![[Pasted image 20240909163431.png]]

미니 포스(Mini POS)는 바자회 축제 또는 임시 카페 운영 시 아날로그 방식의 주문 처리로 인해 발생하는 **비효율적인 주문과 운영 시스템을 개선**할 수 있습니다. **효율적인** 가게 운영과 고객의 만족도 향상을 위해 이 프로젝트를 제안합니다.

이 솔루션은 선불 기반의 번호제 시스템과 현대적인 결제 방식을 통합하여 가게 운영을 최적화하고 수익을 극대화하는 것을 목표로 합니다.

- 선불 기반 번호제 시스템
	- 주문 시 **고유 번호 발급** 및 선불 결제로 주문 확정
	- 효율적인 주문 관리와 가게 순환 개선
- 디지털 주문 관리
	- 실시간 주문 현황 모니터링 자동 정렬
- 간편 결제 시스템
	- 플랫폼 페이 시스템으로 신속하고 정확한 거래 처리
- 주방 운영 최적화
	- 합리적인 제조 순서

## 차별점

![[Pasted image 20240909162858.png]]

## 프로젝트 개발

### 팀원

- 함승우: 메뉴, 주방, QR 및 결제
- 이지수: 앱 디자인, 사용자 가입 및 조회, 메뉴
- 김성진: -

### 문서

- [[2019/Project/MiniPOS/User Interface|User Interface]]
- [[2019/Project/MiniPOS/Database Structure|Database Structure]]
- [[2019/Project/MiniPOS/Flow Chart|Flow Chart]]
#### 기술 스택

| Frontend                                                                                     | Backend                                                                          | Server & Database                                                                              | IDE                                                                            |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| [![My Skills](https://skillicons.dev/icons?i=html,css,jquery)](https://skillicons.dev), ajax | [![My Skills](https://skillicons.dev/icons?i=java)](https://skillicons.dev), jsp | Apache Tomecat 9, [![My Skills](https://skillicons.dev/icons?i=mysql)](https://skillicons.dev) | [![My Skills](https://skillicons.dev/icons?i=eclipse)](https://skillicons.dev) |

- JSP 활용하여 서버 측 로직과 클라이언트 측 인터페이스를 효과적으로 연결 할 수 있게 했습니다.
- AJAX를 통해 비동기 통신으로 사용자 경험 향상했습니다.
- JDBC를 사용하여 Java Application과 MySQL 인터페이스 간 효율적인 데이터 교환할 수 있도록 했습니다.
- 카카오 페이 API를 통해 안전하고 편리한 결제 시스템 구현했습니다.
- Google QR 서비스를 활용해 메뉴판 사이트로 이동할 수 있게 구현했습니다.

## 느낀 점

Github Flow를 활용하지 않고 한 브런치(main)에서 공동 작업했기 때문에 **코드 충돌이** 자주 일어나면서 개발 마일스톤이 지연된 점. 이로 인해 프로젝트 진행에 어려움이 있었습니다.

따라서 Github Flow에 관심을 가지고 분석해봐야 할 것이고, 추후에 다른 SW 프로젝트를 하게 된다면 Github Flow 기법을 적극적으로 활용하여 진행해봐야 할 것입니다.

