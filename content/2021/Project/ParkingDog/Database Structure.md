---
title: Database Structure
description: 
aliases:
  - Seungwoo Ham
tags:
  - document
date: 2024-12-12
---
## CREATE

```sql
CREATE DATABASE IF NOT EXISTS parkingdog
DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

USE parkingdog;
```

```sql
CREATE TABLE IF NOT EXISTS userdata ( -- "가입자 목록"
	id VARCHAR(32) PRIMARY KEY, -- "계정 ID"
	name VARCHAR(32), -- "가입자 이름"
	password VARCHAR(32), -- "계정 비밀번호"
	admin BOOLEAN DEFAULT false, -- "관리자 권한 유무"
	signupts TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- "가입 시간"
);
```

```sql
CREATE TABLE IF NOT EXISTS carpark ( -- "주차장 현황"
	parkname VARCHAR(128) PRIMARY KEY, -- "주차장 이름"
	parkaddress VARCHAR(128), -- "주차장 주소"
	parkspace INT, -- "자릿 수"
	nowspace INT, -- "현재 사용 가능 자릿수"
	canss BOOLEAN, -- "구독 가능 여부"
	ssamount INT, -- "구독 금액"
	owner VARCHAR(32), -- "주차장 주인 ID"
	xlocation DOUBLE(11,8), -- "위도(가로) 좌표"
	ylocation DOUBLE(11,8) -- "경도(세로) 좌표"
);
```

```sql
CREATE TABLE IF NOT EXISTS subscribedata ( -- "구독 현황", 반드시 가입자 목록과 주차장이 먼저 만들어져있어야함
	id VARCHAR(32), -- "계정 ID"
	name VARCHAR(32), -- "가입자 이름"
	startss TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- "구독 시작일"
	endss TIMESTAMP, -- "구독 마감일"
	ssamount INT, -- "구독 금액"
	parkname VARCHAR(128), -- "주차장 이름"
	sid VARCHAR(128), -- "정기결제 코드"
	status BOOLEAN,
	CONSTRAINT id2id FOREIGN KEY (id) REFERENCES userdata (id) ON DELETE CASCADE,
	CONSTRAINT park2park FOREIGN KEY (parkname) REFERENCES carpark (parkname) ON DELETE CASCADE ON UPDATE CASCADE
);
```

```sql
CREATE TABLE IF NOT EXISTS mycar ( -- "내 차" DB
	id VARCHAR(32), -- "계정 ID"
	size VARCHAR(32), -- "차 종 (대형, 중형, 소형)"
	carnumber VARCHAR(32),  -- "차 번호 (ex: 12가 3456)"
	CONSTRAINT connid FOREIGN KEY (id) REFERENCES userdata (id) ON DELETE CASCADE
);
```

```sql
CREATE TABLE IF NOT EXISTS noticeboard ( -- "공지사항"
	number INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
	id VARCHAR(32), -- "작성자 ID"
	name VARCHAR(32), -- "작성자 이름"
	ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- "작성 시각"
	subtype VARCHAR(32), -- "작성 글의 유형"
	title VARCHAR(128), -- "작성 글 제목"
	content VARCHAR(8000), -- "작성 글 내용"
	images VARCHAR(128) -- "이미지"
);
```

```sql
CREATE TABLE IF NOT EXISTS qnaboard ( -- "Q&A"
	number INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
	id VARCHAR(32), -- "작성자 ID"
	name VARCHAR(32), -- "작성자 이름"
	ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- "작성 시각"
	subtype VARCHAR(32), -- "작성 글의 유형"
	title VARCHAR(128), -- "작성 글 제목"
	content VARCHAR(8000), -- "작성 글 내용"
	images VARCHAR(128) -- "이미지"
);
```

```sql
CREATE TABLE IF NOT EXISTS contactUsboard ( -- "문의사항"
	number INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
	id VARCHAR(32), -- "작성자 ID"
	name VARCHAR(32), -- "작성자 이름"
	ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- "작성 시각"
	subtype VARCHAR(32), -- "작성 글의 유형"
	title VARCHAR(128), -- "작성 글 제목"
	content VARCHAR(8000), -- "작성 글 내용"
	images VARCHAR(128) -- "이미지"
);
```

```sql
CREATE EVENT IF NOT EXISTS initializeSSAmount -- 매달 1일마다 구독자 DB의 결제 금액 초기화하는 이벤트
ON SCHEDULE EVERY 1 MONTH STARTS '2021-05-01'
ON COMPLETION PRESERVE ENABLE
DO UPDATE subscribedata SET ssamount = 0;
```

## 시나리오

### 사용자 가입 및 차량 등록

```sql
INSERT INTO userdata (id, name, password) VALUES ('kim_cs', '김철수', 'hashedpassword123');
```

```sql
INSERT INTO mycar (id, size, carnumber) VALUES ('kim_cs', '중형', '12가 3456');
```

### 주차장 검색 및 이용

```sql
SELECT parkname, parkaddress, nowspace, canss, ssamount
FROM carpark
WHERE xlocation BETWEEN ? AND ? AND ylocation BETWEEN ? AND ?;
```

```sql
UPDATE carpark SET nowspace = nowspace - 1 WHERE parkname = '한강뷰 주차장';
```

### 정기구독 신청

```sql
INSERT INTO subscribedata (id, name, endss, ssamount, parkname, sid, status) 
VALUES ('kim_cs', '김철수', DATE_ADD(CURRENT_DATE, INTERVAL 1 MONTH), 100000, '한강뷰 주차장', 'SUB123456', true);
```

### 공지사항 확인

```sql
SELECT title, content, ts FROM noticeboard ORDER BY ts DESC LIMIT 5;
```

### 문의사항 등록

```sql
INSERT INTO contactUsboard (id, name, subtype, title, content)
VALUES ('kim_cs', '김철수', '구독문의', '구독 기간 변경 가능한가요?', '현재 1개월 구독 중인데 3개월로 변경 가능할까요?');
```

### 월별 구독료 초기화

매월 1일, 설정된 이벤트에 의해 구독자들의 월 구독료가 초기화되며,  
`initializeSSAmount` 이벤트가 실행되어 `subscribedata` 테이블의 `ssamount` 컬럼이 초기화됩니다.