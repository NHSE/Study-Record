
# 📘 CHAPTER 2 — Data Type

## 1. Data Type 기본 개념

### 📌 데이터 타입(Data Type)이란?

테이블의 각 컬럼 속에 있는 데이터의 성질

- 숫자형 (Numeric Types)

| Data Type      | 설명             | 크기        | 비고         |
| -------- | -------------- |--------------------|--------------|
|INTERGER        | C/C++ int와 같음    | 4bytes   |             |
|NUMERIC(p,q)    | 소수점자리 표시 가능 | 가변적    |p는 전체 자릿 수, q는 소수점 자릿 수|
|FLOAT           | 부동소수점을 사용    | 4bytes, 8bytes      |         |
|SERIAL          | INTEGER 기본 값으로 1씩 추가되며 값이 자동 생성 | 4bytes      | PRIMARY KEY로 주로 사용|

- 화폐형 (Monetary Types)

| Data Type      | 설명             |
| -------- | -------------- |
|MONEY        | 정수 및 부동 소수점으로 표현 가능    |

- 문자형 (Character Types)

| Data Type      | 설명             |
| -------- | -------------- |
|VARCHAR(n)        | n 이하 문자의 길이 그대로 저장    |
|CAHR(n)        | 문자 길이 + 공백 형태로 n에 맞추어 저장    |
|TEXT        | 길이에 상관없이 모든 문자열을 저장 (= n을 쓰지 않은 VARCHAR)    |

- 날짜 및 시간 (Date & Time)

| Data Type      | 설명             | 크기        |
| -------- | -------------- |--------------------|
|TIMESTAMP        | 현재 세계 표준시(UTC)    | 8bytes   |
|TIMESTAMPTZ        | 세계 표준시 + 시간대 정보 반영 (한국은 GMT + 9)    | 8bytes  |
|DATE        | 날짜 정보만 표시    | 4bytes  |
|TIME        | 시간 정보만 표시, 시계표준시 (시간대 정보 반영 x)    | 8bytes  |
|TIME WITH TIME ZONE       | 시간 정보만 표시, 시계표준시 + 시간대 정보 반영    | 12bytes  |

- 불리언형 (Boolean Types)

| Data Type      | 설명             |
| -------- | -------------- |
|TRUE        | True, yes, on, 1 모두 참    |
|FALSE        | False, no, off, 0 모두 거짓    |
|Null        | 알 수 없는 정보 또는 일부 불확실    |

- 배열형 (Array Types)

| Data Type      | 설명             | 비고         |
| -------- | -------------- |--------------|
|INTEGER[ ]        | INTEGER 형 배열    |'{1, 2}' or Array[1, 2] 로 표현 가능 |
|VARCHAR[ ]        | VARCHAR 형 배열    ||
|BOOLEAN[ ]        | BOOLEAN 형 배열    ||

- 제이슨형 (Json Types)

| Data Type      | 장점             | 단점        | 비고         |
| -------- | -------------- |--------------------|--------------|
|JSON        | 입력한 텍스트의 정확한 사본 생성    | 처리 속도가 느림  | {"키 값" : "Value 값"}            |
|JSONB        | 처리 속도가 비교적 빠름    | 데이터 저장 속도가 느림  |             |

### 📌 Data Type 변경

- CAST 연산자 : 

```sql
CAST (표현식 AS 바꿀 데이터 타입)

/*
ex)
SELECT CAST('2020-08-11' AS TEXT)
*/
```

- CAST 형 연산자

```sql
SELECT 표현식 :: 바꿀 데이터 타입

/*
SELECT '2020-08-11'::TEXT
*/
```

---

## 2. Data Type 무결성

### 📌 무결성이란?

DataBase 내에 정확하고 유효한 데이터만을 유지시키는 속성이며, DBMS 기능 내 제어 기능에 포함됨

1. 개체 무결성 (Entity integrity)

      모든 테이블이 Primary Key를 가져야하며 선택된 컬럼은 고유하고 NULL 값을 허용하지 않아야 한다는 속성


2. 참조 무결성 (Referential integrity)
      
     외래 키(Foreign Key) 값이 null 값이거나 참조된 테이블의 기본 키 값과 동일


3. 범위 무결성 (Domain integrity)

     사용자가 정의한 Domain 내에서 관계형 DataBase의 모든 열을 정의하도록 규정

      ```sql
      CREATE DOMAIN 도메인명 AS 데이터 타입 CHECK (제약 조건)

      /*
      CREATE DOMAIN test AS integer CHECK (VALUE > 0 AND VALUE < 9)
      */
      ```


---

### 📌 Column 값 제한

#### 1) **NOT NULL 제약조건**

* 빈 값을 허용하지 않는 조건
* Null 값이 입력되면 오류 발생

```sql
Column DataType NOT NULL
/*
CREATE TABLE A
(
   id NUMERIC(3) NOT NULL,
   email TEXT NOT NULL
);
*/
```

#### 2) **UNIQUE 제약조건**

* 테이블 내에서 유일한 값을 가짐

```sql
Column DataType UNIQUE NOT NULL
/*
CREATE TABLE A
(
   id NUMERIC(3) UNIQUE NOT NULL,
   email TEXT UNIQUE NOT NULL
);

or

CREATE TABLE A
(
   id NUMERIC(3) NOT NULL,
   email TEXT NOT NULL,
   UNIQUE (id, email)
);
*/
```

#### 3) **PRIMARY KEY**

* 주 식별자, 주 키 또는 줄여서 PK라고 칭함
* PK의 Column 값은 서로 달라야하며, Null 값을 허용하지 않음 ( = UNIQUE NOT NULL)

```sql
Column DataType NOT NULL PRIMARY KEY

/*
CREATE TABLE A
(
   id NUMERIC(3) NOT NULL PRIMARY KEY,
   email TEXT NOT NULL PRIMARY KEY
);

or

CREATE TABLE A
(
   id NUMERIC(3) NOT NULL,
   email TEXT NOT NULL,
   PRIMARY KEY (id, email)
);
*/
```


#### 4) **Foreign Key (외래키)**

* 외래키 제약조건은 다음과 같은 규칙을 따름

   1. 부모 테이블이 자식 테이블 보다 먼저 생성되어야 함.
   2. 부모 테이블은 자식 테이블과 같은 DataType을 가져야 함.
   3. 부모 테이블에서 참조 된 Column 값만 자식 테이블에서 입력 가능
   4. 참조되는 Column은 모두 Primary Key이거나 UNIQUE 제약조건 형식이여야 함

```sql
Column DataType NOT NULL REFERENCES parents_table_name

/*
CREATE TABLE subject
(
   subj_id NUMERIC(5) NOT NULL PRIMARY KEY,
   subj_name VARCHAR(60) NOT NULL
)

INSERT INTO subject VALUES (00001, 'A'), (00002, 'B')

CREATE TABLE teacher
(
   subj_id NUMERIC(5) REFENCES subject
   subj_name VARCHAR(60) REFENCES subject
);

or

// Column명이 다를 경우
CREATE TABLE teacher
(
   id NUMERIC(5) REFENCES subject(subj_id)
   name VARCHAR(60) REFENCES subject(subj_name)
);

// 여러개의 경우
FOREIGN KEY (id, name) REFERENCES subject (subj_id, subj_name)
*/
```

외래키 ON DELETE의 다섯가지 유형

| 유형           | 설명              | 비고  |
|----------------|------------------|------|
| ON DELETE NO ACTION | Default로 설정되어 있으며, 부모 Column을 지울 수 없음 | |
| ON DELETE RESTRICT | 부모 Column을 지울 수 없음 | id NUMERIC(5) REFERENCES A ON DELETE RESTRICT |
| ON DELETE SET NULL | 부모 Column을 지우고 자식 Table 내 해당 Column을 참조한 해당 값들을 모두 NULL로 변경 | id NUMERIC(5) REFERENCES A ON DELETE SET NULL |
| ON DELETE CASCADE | 부모 Column을 지우고 자식 Table 내 해당 Column을 참조한 해당 열을 모두 지움 | id NUMERIC(5) REFERENCES A ON DELETE CASCADE |
| ON DELETE SET DEFAULT | 부모 Column을 지우고 자식 Table 내 해당 Column을 참조한 해당 값들을 모두 설정 값으로 변경 | id NUMERIC(5) DEFAULT 1 REFERENCES A ON DELETE DEFAULT |

#### 5) **CHECK 제약조건**
 * 가장 일반적인 제약 조건이며, CHECK 뒤에 나오는 식이 BOOLEAN 타입의 True를 만족해야함

```sql
CREATE DOMAIN Domain_Name AS DataType CHECK (제약조건)

/*
CREATE DOMAIN A AS integer CHECK (VALUE >= 0 AND VALUE <= 9) --> 0 이상 9 이하의 수 만 입력 가능 조건
*/
```
---

## 3. Alter Table

### 📌 만들어진 테이블에 컬럼 추가

```sql
ALTER TABLE 테이블명 ADD COLUMN 컬럼명 데이터타입 제약조건;

/*
ALTER TABLE a ADD COLUMN A INTEGER NOT NULL;
*/
```

---

### 📌 만들어진 테이블에 컬럼 삭제

```sql
ALTER TABLE 테이블명 DROP COLUMN 컬럼명;
/*
ALTER TABLE a DROP COLUMN A;

or

ALTER TABLE a DROP COLUMN A CASCADE;
*/
```

---
### 📌 만들어진 테이블에 컬럼명 변경

```sql
ALTER TABLE 테이블명 RENAME 기존컬럼명 TO 새컬럼명;
```

---
### 📌 만들어진 테이블에 제약조건 추가 및 제거

```sql
//추가
ALTER TABLE 테이블명 ALTER COLUMN 컬럼명 SET NOT NULL;

//제거
ALTER TABLE 테이블명 ALTER COLUMN 컬럼명 DROP NOT NULL;

//Primary Key 추가
ALTER TABLE 테이블명 ADD PRIMARY KEY (컬럼명);

//외래키 제약조건 추가
ALTER TABLE 자식테이블명 ADD FOREIGN KEY (자식컬럼명) REFERENCES 부모테이블명 (부모컬럼명);
```

---
### 📌 만들어진 테이블에 DataType 변경

```sql
ALTER TABLE 테이블명
ALTER COLUMN 컬럼명 TYPE 새로운 데이터 타입

```
---
