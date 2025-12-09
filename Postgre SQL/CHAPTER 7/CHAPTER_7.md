
# 📘 CHAPTER 7 — 함수와 뷰

## 1. 함수

### 📌 함수란?

함수란, 원하는 목적의 달성을 위해 일련의 SQL문 작업들을 **하나의 단위**로 묶은 것

```sql
CREATE FUNCTION function_name(parameter_1 type, parameter_2 type)
  RETURNS return_type AS
  $$ BEGIN
  ...
  END; $$
LANGUAGE language_name;

/*
ex) 곱셈 함수

CREATE FUNCTION mul(a INTEGER, b INTEGER)
  RETURNS INTEGER
  $$ BEGIN
     RETURN a * b;
  END; $$
LANGUAGE PLpgSQL;

SELECT mul(4, 5);

mul
------
20

*/
```


### 📌 트리거란?

어떠한 행동이나 작업을 했을때 미리 저장해놓은 작업이 **자동으로 실행**되도록 하는 것

```sql
CREATE TRIGGER trigger_name (실행 시기)(옵션) ON 테이블명
FOR EACH ROW EXECUTE PROCEDURE 함수명
```

| 실행시기     | 설명        |
| ------ | ------------ |
| BEFORE  | 쿼리 이벤트 작동하기 후 |
| AFTER | 쿼리 이벤트 작동하기 전 |

| 옵션 | 설명        |
| ------ | ------------ |
| UPDATE | UPDATE 시 실행 |
| INSERT | INSERT 시 실행 |
| DELETE | DELETE 시 실행 |

---

## 2. 뷰

### 📌 뷰란?

뷰는 쿼리문을 저장하고 해당 뷰를 호출할 때 저장된 쿼리문 실행

```sql
// 생성
CREATE VIEW 뷰명 AS 
  <쿼리>;

/*
CREATE VIEW view_name AS
 SELECT column_1
 FROM table_name;

--> view_name에 SELECT column_1 FROM table_name; 라는 쿼리문을 저장함
*/

// 삭제
DROP VIEW 뷰명
```

### 📌 Materialized view (구체화 뷰)
구체화된 뷰는 쿼리문을 저장하고 해당 뷰를 호출할 때 저장된 쿼리문 실행 및 결과 저장

```sql
// 생성
CREATE MATERIALIZED VIEW 뷰명 AS
  <쿼리>
WITH (옵션);

/*
CREATE MATERIALIZED VIEW view_name AS
 SELECT column_1
 FROM table_name
WITH DATA;
*/

// 삭제
DROP MATERIALIZED VIEW 뷰명
```

| 옵션 | 설명        | 비고 |
| ------ | ------------ | --------- |
| DATA | 구체화된 뷰 생성 시 결과 저장 | - |
| NO DATA | 구체화된 뷰 생성 시 결과 저장하지 않음 | `REFRESH MATERIALIZED VIEW no_data_name` 로 결과 값 저장 후 사용|

---
