---
title: Database Structure
description: Simply to pay, Best the cycle 데이터베이스 구조
aliases:
  - Seungwoo Ham
tags:
  - document
date: 2024-09-08
---
## CREATE

```sql
CREATE DATABASE IF NOT EXISTS `minpo` DEFAULT CHARACTER SET utf8mb4 COLLATE  
utf8mb4_general_ci;  
  
USE `minpo`;
```

```sql
CREATE TABLE IF NOT EXISTS `users`  
  (  
     `id`         _VARCHAR_(30) NOT NULL,  
     `password`   _VARCHAR_(50) NOT NULL,  
     `email`      _VARCHAR_(100) NOT NULL,  
     `store_name` _VARCHAR_(50) NOT NULL,  
     PRIMARY KEY (`id`)  
  )  
engine=innodb  
DEFAULT charset=utf8mb4  
COLLATE=utf8mb4_general_ci;  
-- users table
```

```sql
CREATE TABLE IF NOT EXISTS `food_category`  
  (  
     `id`   _INT_ NOT NULL auto_increment,  
     `name` _VARCHAR_(50) NOT NULL,  
     PRIMARY KEY (`id`)  
  )  
engine=innodb  
DEFAULT charset=utf8mb4  
COLLATE=utf8mb4_general_ci;  
-- food category
```

```sql
CREATE TABLE IF NOT EXISTS `food`  
  (  
     `id`       _VARCHAR_(30) NOT NULL,  
     `category` _INT_ NOT NULL,  
     `name`     _VARCHAR_(25) NOT NULL,  
     `price`    _INT_ NOT NULL,  
     KEY `food_userfk` (`id`),  
     KEY `category_foodfk` (`category`),  
     CONSTRAINT `category_foodfk` FOREIGN KEY (`category`) REFERENCES  
     `food_category` (`id`) ON DELETE CASCADE ON UPDATE CASCADE,  
     CONSTRAINT `food_userfk` FOREIGN KEY (`id`) REFERENCES `users` (`id`) ON  
     DELETE CASCADE ON UPDATE CASCADE  
  )  
engine=innodb  
DEFAULT charset=utf8mb4  
COLLATE=utf8mb4_general_ci; 
-- food
```

```sql
CREATE TABLE IF NOT EXISTS `log`  
  (  
     `id`         _VARCHAR_(30) NOT NULL,  
     `order_id`   _INT_ NOT NULL auto_increment,  
     `order_date` _DATETIME_ NOT NULL,  
     `order_log`  _JSON_ NOT NULL,  
     `total`      _INT_ UNSIGNED NOT NULL,  
     `status`     _INT_ UNSIGNED NOT NULL,  
     PRIMARY KEY (`order_id`),  
     KEY `log_userfk` (`id`),  
     CONSTRAINT `log_userfk` FOREIGN KEY (`id`) REFERENCES `users` (`id`) ON  
     DELETE CASCADE ON UPDATE CASCADE  
  )  
engine=innodb  
DEFAULT charset=utf8mb4  
COLLATE=utf8mb4_general_ci;
-- log
```

## 시나리오

### 사용자 등록 및 로그인

```sql
-- 사용자 등록
INSERT INTO users (id, password, email, store_name)  VALUES ('new_user', 'password123', 'user@example.com', 'My Cafe'); 

-- 로그인 (비밀번호 검증) 
SELECT * FROM users  WHERE id = 'new_user' AND password = 'password123';
```

### 메뉴 설정

```sql
-- 카테고리 추가 
INSERT INTO food_category (name) VALUES ('커피'); 

-- 메뉴 추가 
INSERT INTO food (id, category, name, price)  VALUES ('new_user', 1, '아메리카노', 4000);
```

### 주문 접수

```sql
INSERT INTO log (id, order_date, order_log, packaging, total, password, status)  VALUES ('new_user', NOW(), '{"items": [{"name": "아메리카노", "quantity": 2}]}', 0, 8000, '1234', 0);
```

### 주문 처리

```
UPDATE log  SET status = 1  WHERE order_id = 1;
```

### 주문 완료


```sql
UPDATE log  SET status = 2  WHERE order_id = 1;
```

### 매출 분석

```sql
-- 일일 매출 확인 
SELECT DATE(order_date) as date, SUM(total) as daily_total 
FROM log 
WHERE id = 'new_user' 
GROUP BY DATE(order_date); 

-- 인기 메뉴 분석 (JSON 파싱 필요, MySQL 8.0 이상 가정) 
SELECT   f.NAME,  
         _Count_(*) AS order_count  
FROM     log l  
JOIN     json_table(l.order_log, '$.items[*]' columns ( NAME varchar(255) path '$.name' ) ) j  
JOIN     food f  
ON       j.NAME = f.NAME  
WHERE    l.id = 'new_user'  
GROUP BY f.NAME  
ORDER BY order_count DESC;
```

### 메뉴 최적화

```sql
-- 메뉴 가격 변경 
UPDATE food  SET price = 4500  WHERE id = 'new_user' AND name = '아메리카노'; 
-- 새 메뉴 추가 
INSERT INTO food (id, category, name, price)  VALUES ('new_user', 1, '카페라떼', 5000); 
-- 비인기 메뉴 제거 
DELETE FROM food  WHERE id = 'new_user' AND name = '비인기메뉴';
```

### 고객 피드백 (log 테이블에 feedback 컬럼 추가 가정)

```sql
-- 피드백 기록 
UPDATE log  SET feedback = '맛있고 서비스가 좋았습니다.'  WHERE order_id = 1; 
-- 피드백 분석 
SELECT feedback, COUNT(*) as count  FROM log  WHERE id = 'new_user'  GROUP BY feedback;
```
