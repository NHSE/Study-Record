
# 📘 Problem 4

이번 문제는 모두를 위한 PostgreSQL에서 제공하는 소스코드를 활용함 (https://github.com/bjpublic/postgresql)

```sql
createdb -U postgres population_and_example
psql -U postgres -d population_and_example-f <경로>/population_and_example.dump
```

- accident 테이블 : 2019년 한해동안 사고가 발생한 내역을 시도, 시군구 정보와 사고에 대한 정보를 담고 있음
- popluation 테이블 : 2019년의 시도, 시군구별 세대수와 남/여 인구수에 대한 정보를 담고 있음
- road_type 테이블 : 사고가 발생할 수 있는 도로 형태에 대한 정보를 담고 있음

## 문제 1

교차로에서 사고자(다치거나 사망한 사람)가 5명 이상인 대형 사고는 어느 시도, 시군구에서 발생했는지 사고자 수로 내림차순하여 출력하라.

<details>
    <summary>정답</summary>

```sql
SELECT 시도, 시군구, count(*) AS 사고횟수
FROM accident JOIN road_type USING(도로형태id)
WHERE road_type.대분류 = '교차로'
   AND (사망자수 + 부상자수 + 중상자수 + 경상자수 + 부상신고자수) >= 5
GROUP BY 시도, 시군구
ORDER BY 사고횟수 DESC;
```

</details>

---

## 문제 2

세대수 대비 사망자수가 많은 순으로 시도, 시군구를 출력하라.

<details>
    <summary>정답</summary>

```sql
SELECT population.시도, population.시군구, 시군구별_사고.사망자수::decimal/population.세대수 AS 세대당_사망자수
FROM population
JOIN (
   SELECT 시군구, 시도, sum(사망자수) AS 사망자수
   FROM accident
   GROUP BY 시군구, 시도
   ORDER BY 사망자수 DESC
) 시군구별_사고
ON 시군구별_사고.시군구 = population.시군구
   AND 시군구별_사고.시도 = population.시도
ORDER BY 세대당_사망자수 DESC;
```

</details>

---

## 문제 3.

광역단체(시도)의 각 시군구 평균 사고 횟수 대비 각 시군구가 얼만큼 사고가 더 나는지 증감을 표로 표시하여라.

<details>
    <summary>정답</summary>

```sql
SELECT 시군구_사고횟수.시도, 시군구_사고횟수.시군구,
   시군구_사고횟수.사고횟수,
   (시군구_사고횟수.사고횟수 -  시도_평균_사고횟수.평균_사고횟수)
   AS 평균대비_사고횟수_증감
FROM (
   SELECT 시군구, 시도, count(*) AS 사고횟수
   FROM accident
   GROUP BY 시군구, 시도
) 시군구_사고횟수 JOIN
(
  SELECT 시도, avg(사고횟수) AS 평균_사고횟수
  FROM (
     SELECT 시군구, 시도, count(*) AS 사고횟수
     FROM accident
     GROUP BY 시군구, 시도
 ) 시군구_사고횟수
 GROUP BY 시도
) 시도_평균_사고횟수
ON 시군구_사고횟수.시도 = 시도_평균_사고횟수.시도
ORDER BY 평균대비_사고횟수_증감 DESC;
```

</details>

---