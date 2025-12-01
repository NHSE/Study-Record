---

# 📘 CHAPTER 1 — Database & PostgreSQL

## 1. Database 기본 개념

### 📌 데이터(Data)란?

온도, IQ, 가격처럼 **그 자체로 단순한 사실을 나타내는 값**을 의미.

### 📌 데이터베이스(Database)란?

데이터를 **논리적으로 체계화하여 여러 사용자가 공유하고 사용할 목적**으로 통합한 집합.

**데이터베이스의 특징**

1. **실시간 접근성**
   비정형적 조회에 대해서도 즉시 응답해야 한다.
2. **지속적인 변화**
   데이터의 Insert / Delete / Update로 항상 최신 상태 유지.
3. **동시 공유**
   다수의 사용자가 동시에 동일한 데이터를 사용할 수 있어야 한다.
4. **내용 기반 참조**
   주소가 아닌 “내용”을 기준으로 원하는 데이터를 검색한다.

---

### 📌 DBMS(Database Management System)

사용자와 Database 사이에서 **중간 다리 역할**을 하며, 데이터 통합 관리 기능을 제공하는 소프트웨어.

**DBMS 기능**

* **정의 기능**: DB 구조 생성·변경·삭제
* **조작 기능**: 데이터 삽입, 갱신, 삭제
* **제어 기능**: 접근 권한 관리, 동시성 제어, 무결성 유지

---

### 📌 데이터베이스 모델 종류

#### 1) **계층형 Database**

* 부모–자식 구조
* 검색 속도 빠름
* 다대다 구조 표현 불가

#### 2) **네트워크형 Database**

* 다대다 관계 지원
* 계층형의 단점 보완
* IDS, IDMS 등에서 사용

#### 3) **관계형 Database(RDB)**

* 2차원 테이블(행/열) 구조
* SQL 사용
* EX) Excel과 동일한 형태

---

## 2. PostgreSQL

### 📌 PostgreSQL 구조

(사진 삽입 위치)

* 프로세스 구조는 **클라이언트-서버 모델 기반**
* 클라이언트와 서버는 인터페이스 라이브러리를 통해 통신

---

### 📌 PostgreSQL 계층 구조

(사진 삽입 위치)

* **Cluster** : Database들의 집합
* **Schema** : 테이블·뷰·함수·인덱스 등 개체들의 논리적 그룹
* **Table** : Row(행) + Column(열)

---

## 3. SQL

### 📌 SQL이란?

Database에 접근하여 데이터를 다루는 **전용 언어(Structured Query Language)**

### 📌 SQL 유형

#### 1) **데이터 정의어(DDL: Data Definition Language)**

객체 생성/변경/삭제

* `CREATE`, `ALTER`, `DROP`, `RENAME`, `TRUNCATE`

#### 2) **데이터 조작어(DML: Data Manipulation Language)**

데이터 삽입/조회/수정/삭제

* `SELECT`, `INSERT`, `UPDATE`, `DELETE`

#### 3) **데이터 제어어(DCL: Data Control Language)**

권한 관리

* `GRANT`, `REVOKE`

---

# psql Shell 사용법

### 📌 psql 접속

```bash
psql -U postgres
```

### 📌 주요 명령어

| 명령어      | 설명             |
| -------- | -------------- |
| `\q`     | psql 종료        |
| `\l`     | 데이터베이스 목록 조회   |
| `\c DB명` | 특정 DB로 이동      |
| `\e`     | 외부 편집기에 SQL 작성 |
| `\dt`    | 테이블 목록 조회      |

---

# 📌 기본 SQL 문법 정리

## 1. Database 생성 / 삭제

```sql
CREATE DATABASE DB;
```

```sql
DROP DATABASE DB;
```

## 2. 테이블 생성 / 삭제

```sql
CREATE TABLE 테이블명 (
    컬럼명 자료형,
    컬럼명 자료형
);
```

```sql
DROP TABLE 테이블명;
```

---

## 3. 데이터 추가

### 🟦 컬럼 순서대로 삽입

```sql
INSERT INTO 테이블명 VALUES
('데이터1', '데이터2', '데이터3'),
('데이터4', '데이터5', '데이터6');
```

### 🟦 특정 컬럼 지정 삽입

```sql
INSERT INTO 테이블명 (A, B, C) VALUES
('데이터1', '데이터2', '데이터3');
```

---

## 4. 데이터 조회

### 전체 조회:

```sql
SELECT * FROM 테이블명;
```

### 특정 컬럼 조회:

```sql
SELECT A, B FROM 테이블명;
```

---

## 5. 조회 옵션

### 📌 LIMIT
반환하는 row의 개수를 지정
```sql
SELECT * FROM 테이블명
LIMIT 3;
```

### 📌 OFFSET
반환하는 row의 시작점을 지정
```sql
SELECT * FROM 테이블명
LIMIT 3 OFFSET 1;
```
→ 0-Base 기준 1에서부터 3개 데이터 조회

→ 인덱스 1(row 2)부터 3개 조회

### 📌 ORDER BY
반환하는 row를 정렬할 때 사용
```sql
SELECT * FROM 테이블명
ORDER BY 컬럼명 ASC/DESC;
```
→ ASC : 해당 컬럼을 오름차 순

→ DESC : 해당 컬럼을 내림차 순

다중 컬럼 정렬:

```sql
SELECT A, B FROM 테이블명
ORDER BY 2, 1;
```
→ B 컬럼이 먼저 정렬된 후 A의 컬럼이 정렬

→ 문자열의 경우 유니코드상의 순서로 정렬

### 📌 WHERE 조건
지정한 로우만 조회가 되도록 필터 기능

| 연산자 | 의미       |
| --- | -------- |
| =   | 같다       |
| <>  | 다르다      |
| >   | 왼쪽이 더 큼  |
| <   | 오른쪽이 더 큼 |
| >=  | 크거나 같다   |
| <=  | 작거나 같다   |

---

## 6. 서브쿼리

```sql
SELECT * FROM 테이블명
WHERE A = (
    SELECT 컬럼명
    FROM 테이블명
    WHERE pkey = 3
);
```
→ 서브쿼리 내 pkey 3번의 이름 컬럼이 A의 경우 메인 쿼리문의 WHERE가 참이 되므로 'SELECT * FROM 테이블명'가 실행됨

---

## 7. 데이터 수정 (UPDATE)

```sql
UPDATA 테이블명
SET 컬럼명 = 바꿀 데이터 내용
WHERE 수정할 row의 조건
```

```sql
ex)
UPDATA 테이블명
SET A = 'B'
WHERE pkey = 3;
```
→ 3번 pkey의 A라는 컬럼의 데이터 값을 'B'로 변경

→ DataBase는 최근 수정된 row를 가장 마지막에 출력하므로 정렬 후 사용

---

## 8. AS (별칭)

### 📌 출력 컬럼명 변경

```sql
SELECT A AS 이름, B AS 값
FROM 테이블명;
```

### 📌 테이블 복사

```sql
CREATE TABLE 새테이블 AS
SELECT * FROM 기존테이블;
```

### 📌 테이블명 변경

```sql
ALTER TABLE 기존명 RENAME TO 새이름;
```

---

## 9. 데이터 삭제

### 특정 행 삭제

```sql
DELETE FROM 테이블명
WHERE 컬럼명 = '삭제값';
```

### 전체 데이터 삭제

```sql
DELETE FROM 테이블명;
```

---
