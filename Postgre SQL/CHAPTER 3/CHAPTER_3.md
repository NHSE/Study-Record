
# 📘 CHAPTER 3 — 연산자와 함수

## 1. 연산자와 조건문 함수

### 📌 논리 연산자, 비교 연산자, 범위 연산자

AND, OR, NOT으로 구성되어 있으며 True, False, Null 3개의 결과 값을 이루어져 있음

- 논리 연산자

| A      | B             | A AND B        | A OR B         | NOT A |
| -------- | -------------- |--------------------|--------------|-----|
|T | T | T | T | F |
|T | F | F | T | F |
|T | NULL | NULL | T | F |
|F | F | F | F | T |
|F | NULL | F | NULL | T |
|NULL | NULL | NULL | NULL | NULL|

- 비교 연산자

| 연산자      | 설명             | 결과        |
| -------- | -------------- |--------------------|
|<불리언 표현식> IS TRUE        | <불리언 표현식>이 참이다.    | 참 또는 거짓   |
|<불리언 표현식> IS NOT TRUE    | <불리언 표현식>이 참이 아니다.    | 참 또는 거짓  |
|<불리언 표현식> IS FALSE       | <불리언 표현식>이 거짓이다.    | 참 또는 거짓  |
|<불리언 표현식> IS NOT FALSE   | <불리언 표현식>이 거짓이 아니다.    | 참 또는 거짓  |
|<불리언 표현식> IS NULL        | <불리언 표현식>이 NULL이다.    | 참 또는 거짓  |
|<불리언 표현식> IS NOT NULL    | <불리언 표현식>이 NULL이 아니다.    | 참 또는 거짓  |

```sql
SELECT * FROM 테이블명 WHERE A IS FALSE;
```


- 범위 연산자

| 연산자      | 설명             |
| -------- | -------------- |
|BETWEEN        | A 이상 B 이하의 범위    |
|NOT BETWEEN    | A 초과 B 미만의 범위    |

```sql
// (1 <= A AND A <= 10)
SELECT * FROM 테이블명 WHERE BETWEEN 1 AND 9;

// (1 < A AND A < 10)
SELECT * FROM 테이블명 WHERE NOT BETWEEN 1 AND 9;
```

---

### 📌 조건문 함수

- CASE : IF-ELSE 문과 대응되며 C/C++의 swtich-case문과 동일

```sql
CASE
    WHEN <조건문> THEN <결과문>
    WHEN <조건문> THEN <결과문>
    ELSE <결과문>
END

/*
ex)
CASE
    WHEN score <= 100 AND score >= 90 THEN 'A'
    WHEN score <= 89 AND score >= 80 THEN 'B'
    WHEN score <= 79 AND score >= 70 THEN 'C'
    ELSE 'F'
END grade
*/
```

- COALESCE : NULL 값을 다른 기본 값으로 대체될 때 사용

```sql
COALESCE(컬럼1, 값1);

/*
COALESCE(score, 0),
CASE
    WHEN score <= 100 AND score >= 90 THEN 'A'
    WHEN score <= 89 AND score >= 80 THEN 'B'
    WHEN score <= 79 AND score >= 70 THEN 'C'
    ELSE 'F'
END grade

--> score가 NULL 값이라면 0으로 변경
*/
```

- NULLIS : 값 1, 값 2가 같을 경우 NULL 반환, 다를 경우 값 1 반환

```sql
NULLIF (값 1, 값 2);

/*
SELECT students, COALESCE((12/NULLIF(students, 0))::char, '나눌 수 없음') AS share
FROM A;

--> NULLIF의 값 1과 값 2가 같을 경우 NULL을 반환하여 COALESCE의 두번째 값을 share에 저장
*/
```

---

## 2. 배열 연산자와 함수

### 📌 배열 연산자

| 연산자 | 설명 | 예시 | 결과 |
|-----|-----|-----|-----|
| <@ 또는 @>        | 포함관계    | ARRAY[1, 2, 3] @> ARRAY[1, 3]   | 참 |
| &&    | 겹침 유/무    | ARRAY[1, 2, 3, 4] && ARRAY[1, 5, 6] | 참 |

### \|\| 연산자
| 설명 | 예시 | 결과 |
|------|------|-------|
| 배열끼리 병합 | ARRAY[1,2,3] \|\| ARRAY[1,3] | {1,2,3,1,3} |

| 설명 | 예시 | 결과 |
|------|------|-------|
| 2차원 배열 병합 | ARRAY[[1,2,3],[4,5,6]] \|\| ARRAY[7,8,9] | {{1,2,3},{4,5,6},{7,8,9}} |

| 설명 | 예시 | 결과 |
|------|------|-------|
| 원소 배열 병합 | 1 \|\| ARRAY[2,3,4] | {1,2,3,4} |


### 📌 배열 함수

| 함수 | 설명 | 예시 | 결과 |
|------|------|-------|------|
| array_append() | 배열에 맨 뒤에 원소를 추가 | array_append(ARRAY[1, 2], 3) | {1, 2, 3} |
| array_prepend() | 배열에 맨 앞에 원소를 추가 | array_prepend(1, ARRAY[2, 3]) | {1, 2, 3} |
| array_remove() | 배열의 특정 원소를 삭제 | array_remove(ARRAY[1, 2], 1) | { 2 } |
| array_replace() | 배열의 특정 원소를 다른 원소와 대체 | array_replace(ARRAY[1, 3], 3, 2) | {1, 2} |
| array_cat() | 두 배열을 병합 | array_cat(ARRAY[1, 2], ARRAY[3, 4]) | {1, 2, 3, 4} |

## 3. JSON 연산자와 함수

### 📌 JSON 연산자

| 연산자      | 설명             | 결과        |
| -------- | -------------- |--------------------|
|->        |JSON 오브젝트에서 키 값으로 밸류 값을 불러오기 or JSON 배열에서 인덱스로 JSON 오브젝트 불러오기    | JSON |
|->>    | JSON 오브젝트, JSON 배열 속 데이터 텍스트로 불러오기    | TEXT |
|#>       | 특정한 경로의 값을 가져옴    | JSON |
|#>>   | 특정한 경로의 값을 TEXT 데이터 타입으로 가져옴    | TEXT |


```sql

// ->
SELECT '{"p": {"1" : "postgres"}, "s" : {"1" : "sql"}}'::json -> 'p' AS result;

result
---------------
{"1" : "postgres"}

Type : Json

// ->>
SELECT '{"p": {"1" : "postgres"}, "s" : {"1" : "sql"}}'::json ->> 'p' AS result;

result
---------------
{"1" : "postgres"}

Type : TEXT

// #>
SELECT '{"post": [{"gre": {"sql":"do it"}}, {"t":"sql"}]}'::json #> '{"post",1,"t"}' AS result;

result
---------------
"sql"

Type : Json

// #>>
SELECT '{"post": [{"gre": {"sql":"do it"}}, {"t":"sql"}]}'::json #>> '{"post",1,"t"}' AS result;

result
---------------
"sql"

Type : TEXT

```

---

### 📌 JSON 생성함수

| 연산자      | 설명             |
| -------- | -------------- |
|json_build_object()        | JSON 오브젝트 생성 함수    |
|json_build_array()    | JSON 배열 생성 함수    |
|json_array_length()       | 배열의 원소의 개수 체크 함수    |
|json_array_elements()   | 배열의 원소를 컬럼으로 출력    |
|json_each()   | 키 값과 밸류 값을 기준으로 출력    |

```sql
// json_build_object()
SELECT json_build_object('a', 1, 'b', 2) AS result;

result
---------------
{"a" : 1, "b" : 2}

Type : JSON

// json_build_array()
SELECT json_build_array('a', 1, 'b', 2) AS result;

result
---------------
["a", 1 , "b", 2, "c"]

Type : JSON

// json_array_length()
SELECT json_array_length('["a", 1, "b", 2, "c"]'::json) AS result;

result
---------------
5

Type : Integer

// json_array_elements()
SELECT json_array_elements('[1,"a",{"b":"c"}, ["d", 2, 3]]') AS result;

result
---------------
1
"a"
{"b":"c"}
["d", 2, 3]

Type : JSON

/*
SELECT comments->'username' AS username, comments->'contents' AS contests 
FROM 
     json_array_elements('[{"username" : "전우치", "contents" : "댓글 내용"}, 
                           {"username" : "서화담", "contents" : "댓글 내용"}]') comments;


*/
username | contests
---------+----------
"전우치" | "댓글 내용"
"서화담" | "댓글 내용"

// json_each()
SELECT * FROM json_each('{"sujin" : "i like postgresql", "Siyoun":"i like postgresql too"}');

 Key     | Value
---------+----------
"sujin"  | "i like postgresql"
"Siyoun" | "i like postgresql too"
```

---
## 4. 자주 쓰이는 연산자와 함수
### 📌 서브쿼리 연산자

| 연산자      | 설명             | 결과        |
| -------- | -------------- |--------------------|
|EXISTS(서브쿼리)        | 서브쿼리의 로우가 존재하면 참    | 참 또는 거짓, NULL |
|<표현> IN (서브쿼리)    | 서브쿼리의 로우 값 중 하나라도 표현식과 같다면 참 | 참 또는 거짓, NULL |
|<표현> NOT IN (서브쿼리)       | 서브쿼리의 로우 값 중 하나라도 다르면 표현식과 같다면 참 | 참 또는 거짓, NULL |
|<표현> <비교 연산자> ANY (서브쿼리)   | 서브쿼리의 로우 값 중 하나라도 표현식과 같다면 참 | 참 또는 거짓, NULL |
|<표현> <비교 연산자> ALL (서브쿼리)   | 서브쿼리의 로우 값 모두가 표현식과 같다면 참 | 참 또는 거짓, NULL |

```sql
// EXISTS
SELECT * FROM A
WHERE EXISTS ( SELECT * FROM B );

// --> B 테이블 내 데이터 존재 시 A 테이블 출력
// --> B 테이블 내 데이터 존재하지 않을 경우 출력되지 않음

// IN
SELECT * FROM A
WHERE A IN ( 10, 20, 30 );

-------
id | name | amount
---+-------+-------
1  | apple | 30
2  | melon | 10
3  | kiwi  | 30

// --> A 테이블 내 10, 20, 30의 값을 가진 로우만 출력

// NOT IN
SELECT * FROM A
WHERE A NOT IN ( 10, 20, 30 );

-------
id |  name      | amount
---+------------+-------
4  | banana     | 23
5  | strawberry | 43

// --> A 테이블 내 10, 20, 30의 값을 가지지 않은 로우만 출력

// ANY
SELECT * FROM A
WHERE 10 = ANY ( SELECT b FROM B );

// --> B 테이블 내 b(컬럼) 중 10이라는 로우 존재 시 True, 없을 경우 False 반환

// ALL
SELECT * FROM A
WHERE 10 <= ALL ( SELECT b FROM B );

// --> B 테이블의 b 컬럼 로우 값이 모두 10보다 크거나 같을 경우 True, 다를 경우 False 반환
```

---
