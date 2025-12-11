
# 📘 Problem 2

다음 모델을 바탕으로 데이터 베이스를 설계할 것이다.

<img width="1599" height="1187" alt="image" src="https://github.com/user-attachments/assets/6a8bad1f-473d-42ff-a6f7-2c47eb1a88f1" />

## 문제 1. 국가 테이블 생성

상기의 그림을 보고 국가 테이블을 만들어라.

<details>
    <summary>정답</summary>

```sql
CREATE TABLE country (
country_cd SERIAL NOT NULL PRIMARY KEY,
country_name VARCHAR(30) NOT NULL,
region VARCHAR(30) NOT NULL);
```

</details>

---

## 문제 2. 도메인 생성

pwchar라는 도메인이 필요하다. pwchar 도메인은 char 형식으로 글자수가 8자 이상 16자 이하인 문자열이다. 

추가적으로 char_length 함수를 사용하면 문자열의 개수로 반환한다.

<details>
    <summary>정답</summary>

```sql
CREATE DOMAIN pwchar AS char
CHECK( 8 <= char_length(VALUE) AND char_length(VALUE) <= 16);
```

</details>

---

## 문제 3. 유저 테이블 생성

상기의 그림을 보고 유저 테이블을 만들어라.

<details>
    <summary>정답</summary>

```sql
CREATE TABLE users (
user_pk SERIAL NOT NULL,
user_id VARCHAR(30) NOT NULL UNIQUE,
user_pw pwchar NOT NULL,
user_age NUMERIC(3) NOT NULL,
register_date DATE NOT NULL,
country_cd SERIAL NOT NULL REFERENCES country,
PRIMARY KEY (user_pk, country_cd)
);

/*
REFERENCES 시 생성 테이블과 참조 테이블 내 컬럼명이 같을 경우 자동으로 연결됨
*/
```

</details>

---

## 문제 4. 주제 테이블 생성

상기의 그림을 보고 주제 테이블을 만들어라.

<details>
    <summary>정답</summary>

```sql
CREATE TABLE topic (
topic_cd SERIAL NOT NULL PRIMARY KEY,
topic_name VARCHAR(100) NOT NULL UNIQUE
);
```

</details>

---

## 문제 5. 게시판 테이블 생성
상기의 그림을 보고 게시판 테이블을 만들어라.

이때 BOARD_ID는 유저 테이블의 USER_ID를 TOPIC_CD는 주제 테이블을 참조한다.

<details>
    <summary>정답</summary>

```sql
CREATE TABLE board (
board_pk SERIAL NOT NULL,
board_user VARCHAR(30) NOT NULL UNIQUE,
board_date DATE NOT NULL,
topic_cd SERIAL NOT NULL,
title VARCHAR(100) NOT NULL,
description TEXT NOT NULL,
hidden BOOLEAN NOT NULL,
likes INTEGER NOT NULL,
comments JSON,
PRIMARY KEY (board_pk, topic_cd),
FOREIGN KEY (board_user) REFERENCES users (user_id),
FOREIGN KEY (topic_cd) REFERENCES topic
);
```

</details>

---
