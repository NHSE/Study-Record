
# 📘 CHAPTER 5 — 데이터의 집계 및 결합 Part.2

## 1. 여러 개의 테이블 로우로 연결하기

### 📌 UNION, UNION ALL

UNION : 중복을 제거하고 로우를 결합

UNION ALL : 중복을 제거하지 않고 로우을 결합

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/5aa4fc96-8173-42e5-8aab-307e0af0450d" />

```sql
SELECT * FROM 테이블명 UNION SELECT * FROM 테이블명

SELECT * FROM 테이블명 UNION ALL SELECT * FROM 테이블명
/*
A | item_column_1
  |       a
  |       b
  |       c

B | item_column_2
  |       a
  |       b

SELECT * FROM A UNION SELECT * FROM B

result
----------------
a
b
c

SELECT * FROM A UNION ALL SELECT * FROM B

result
----------------
a
a
b
b
c

*/
```

---

### 📌 INTERSECT, INTERSECT ALL

INTERSECT : 중복되는 로우 한번만 출력

INTERSECT ALL : 중복되는 로우 모두 출력

<img width="317" height="329" alt="image" src="https://github.com/user-attachments/assets/0e3be24d-9027-433d-8978-47c49e5c660a" />

```sql
(SELECT 컬럼명 FROM 테이블명) INTERSECT (SELECT 컬럼명 FROM 테이블명)

(SELECT 컬럼명 FROM 테이블명) INTERSECT ALL (SELECT 컬럼명 FROM 테이블명)

/*
--------
A | item_column_1
  |       a
  |       b
  |       c

B | item_column_2
  |       a
  |       b

(SELECT item_column_1 FROM A) INTERSECT (SELECT item_column_2 FROM B)

result
-----------
a
b

(SELECT item_column_1 FROM A) INTERSECT ALL (SELECT item_column_2 FROM B)

result
-----------
a
a
b
b
*/
```

### 📌 EXCEPT, EXCEPT ALL

EXCEPT : 두 테이블의 로우 정보 중 중복되지 않은 부분만을 중복없이 출력

EXCEPT ALL : 두 테이블의 로우 정보 중 중복되지 않은 부분만을 그대로 출력

<img width="317" height="327" alt="image" src="https://github.com/user-attachments/assets/513296be-af37-4415-bf33-4fdda18b5004" />

```sql
(SELECT 컬럼명 FROM 테이블명) EXCEPT (SELECT 컬럼명 FROM 테이블명)

(SELECT 컬럼명 FROM 테이블명) EXCEPT ALL (SELECT 컬럼명 FROM 테이블명)

/*
--------
(SELECT item_column_1 FROM A) EXCEPT (SELECT item_column_2 FROM B)

(SELECT item_column_1 FROM A) EXCEPT ALL (SELECT item_column_2 FROM B)

*/
```

## 2. 여러 개의 테이블 컬럼으로 연결하기

### 📌 FROM과 WHERE을 이용한 데이터 결합

두 테이블의 각각의 로우가 서로 한번씩 결합시키는 교차 조인하여 WHERE로 원하는 조건을 통해 출력

```sql

SELECT * FROM 테이블명1, 테이블명2
WHERE <조건문>

/*
A |   id
  |    a
  |    b
  |    c

B | item_id | item_type
  |    a    |   'asd'
  |    b    |   'dsa'

SELECT * FROM A, B
WHERE A.id = B.item_id AND B.item_type = 'asd'

id | item_id | item_type
---+---------+-----------
a  |    a    |   'asd'
*/
              
```

### 📌 JOIN을 이용한 데이터 결합
FROM - WHERE을 JOIN - ON으로 변경 가능

<img width="808" height="348" alt="image" src="https://github.com/user-attachments/assets/5f346c2e-5fc1-4a90-806e-0d32b972f9c0" />

```sql
SELECT * FROM A, B
WHERE A.id = B.item_id AND B.item_type = 'asd'

->

SELECT * FROM 
( 
   A JOIN B
   ON A.id = B.item_id AND B.item_type = 'asd'
);
```

| 함수 | 축약형 | 내부/외부 구분 |
|------|------|-------|
| INNER JOIN | JOIN | 내부 |
| LEFT OUTER JOIN | LEFT JOIN | 외부 |
| RIGHT OUTER JOIN | RIGHT JOIN | 외부 |
| FULL OUTER JOIN | FULL JOIN | 외부 |

```sql

SELECT * FROM 
( 
   테이블명1 JOIN or LEFT JOIN or RIGHT JOIN or FULL JOIN 테이블명2
   ON <조건문>
);

/*
A |   id
  |    a
  |    b
  |    c

B | item_id | item_type
  |    a    |   'asd'
  |    b    |   'asd'
  |    e    |   'dsa'

// JOIN
SELECT * FROM 
( 
   A JOIN B
   ON A.id = B.item_id AND B.item_type = 'asd'
);

id | item_id | item_type
---+---------+-----------
a  |    a    |   'asd'
b  |    b    |   'asd'

// LEFT JOIN
SELECT * FROM 
( 
   A LEFT JOIN B
   ON A.id = B.item_id AND B.item_type = 'asd'
);

id | item_id | item_type
---+---------+-----------
a  |    a    |   'asd'
b  |    b    |   'asd'
c  |   null  |    null

// RIGHT JOIN
SELECT * FROM 
( 
   A RIGHT JOIN B
   ON A.id = B.item_id AND B.item_type = 'asd'
);

id    | item_id | item_type
------+---------+-----------
a     |    a    |   'asd'
b     |    b    |   'asd'
null  |    e    |   'dsa'

// FULL JOIN
SELECT * FROM 
( 
   A FULL JOIN B
   ON A.id = B.item_id AND B.item_type = 'asd'
);

id    | item_id | item_type
------+---------+-----------
a     |    a    |   'asd'
b     |    b    |   'asd'
c     |   null  |    null
null  |    e    |   'dsa'

*/
```
---
