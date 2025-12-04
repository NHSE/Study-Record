
# 📘 CHAPTER 3 — 데이터의 집계 및 결합 Part.1

## 1. 데이터 그룹화

### 📌 DISTINCT

테이블에서 중복되는 데이터를 없애는 명령어

```sql
SELECT DISTINCT 중복을 제거할 컬럼 지정
FROM 테이블명

/*
SELECT DISTINCT item_type, item_id --> item_type과 item_id가 동일한 컬럼 제거
FROM table
ORDER BY item_type;
*/
```

---

### 📌 GROUP BY

```sql
SELECT 그룹화 할 컬럼 FROM 테이블명
GROUP BY 그룹화 할 컬럼


/*
SELECT item_column_1, item_column_2 FROM A
GROUP BY item_column_1, item_column_2;
*/
```

### 📌 HAVING
HAVING은 집계된 데이터를 조회할 경우 사용됨.

WHERE의 경우 집계 전 데이터를 조회할 때 사용하는 차이가 있음

```sql
SELECT 그룹화 할 컬럼 FROM 테이블명
GROUP BY 그룹화 할 컬럼
HAVING <조건문>

/*
SELECT item_column_1, item_column_2 FROM A
GROUP BY item_column_1, item_column_2;
HAVING count > 3

--> 집계된 데이터의 수가 3 초과의 경우에만 출력
*/
```

### 📌 SQL 명령 우선순위

| 명령어 | 설명 | 우선순위 |
|------|------|-------|
| FROM | 불러올 테이블 지정 | 1 |
| WHERE | 지정 테이블에서 조건을 통해 데이터를 가져옴 | 2 |
| GROUP BY | 지정한 컬럼을 하나의 그룹으로 묶음 | 3 |
| HAVING | 집계된 컬럼 데이터를 필터링 | 4 |
| SELECT | 조회할 컬럼을 선택 | 5 |
| DISTINCT | 컬럼에서 중복된 데이터를 제외 | 6 |
| ORDER BY | 지정된 컬럼을 오름차순/내림차순으로 정렬 | 7 |
| LIMIT | 지정한 수만큼 조회할 컬럼을 제한 | 8 |

- 명령어의 우선순위는 실행 순서이며, 코드 작성 순서와는 상관없음.
---

## 2. 집계 함수

### 📌 기본적인 집계 함수

| 함수 | 출력내용 | 예시 |
|------|------|-------|
| avg() | null값이 아닌 모든 입력 값의 평균 | avg(item_column) |
| count(*) | 입력한 로우의 총 개수 | count(*) |
| count() | null값이 아닌 모든 입력 로우 값의 개수 | count(item_column) |
| max() | null값이 아닌 모든 입력 값의 최댓값 | max(item_column) |
| min() | null값이 아닌 모든 입력 값의 최솟값 | min(item_column) |
| sum() | null값이 아닌 모든 입력 값의 합 | sum(item_column) |

```sql
// 해당 집계 값을 가지는 Column을 조회하는 방법
SELECT column FROM A
WHERE score = 
(
    SELECT max(score) FROM A
);

// --> A 테이블 내 score 값이 max인 부분을 서브쿼리로 확인하고 해당 값과 현재 로우의 score 값이 일치하는 지 확인 후 출력
              
```


### 📌 불리언 연산 집계 함수

| 함수 | 출력내용 | 예시 |
|------|------|-------|
| bool_and() | 입력된 데이터가 모두 참이면 참을 출력 | bool_and(bool_column) |
| bool_or() | 입력한 데이터 중 하나라도 참이면 참을 출력 | bool_or(bool_column) |
| every() | = bool_and() | count(item_column) |

### 📌 배열을 담는 집계 함수

| 함수 | 출력내용 | 예시 |
|------|------|-------|
| array_agg() | 배열로 연결된 null 값을 포함한 입력 값, 더 높은 차원의 배열로 연결된 입력 배열 | array_agg(column) |

```sql
SELECT array_agg(name) FROM A

A
----------
{1, 2, 3, 4, "a"}

```

### 📌 JSON 집계 함수

| 함수 | 출력내용 |
|------|------|
| json_agg() | null을 포함해 json 배열로 집계한 값 |
| jsonb_agg() | null을 포함해 jsonb 배열로 집계한 값 |
| json_object_agg(name, value) | name-value 쌍을 json 개체로 집계 한 값, value는 null 포함, name은 포함하지 않음 |
| jsonb_object_agg(name, value) | name-value 쌍을 jsonb 개체로 집계 한 값, value는 null 포함, name은 포함하지 않음 |

---