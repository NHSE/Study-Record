
# 📘 Problem 3

이번 문제는 모두를 위한 PostgreSQL에서 제공하는 소스코드를 활용함 (https://github.com/bjpublic/postgresql)

```sql
createdb -U postgres gyeonggi_graduates
psql -U postgres -d gyeonggi_graduates -f <경로>/gyeonggi_graduates.dump
```

## 문제 1

한해 동안 특수학교에서 졸업한 학생의 수가 25명 이상이었던 학교 이름과 남여 졸업생 수를 출력하라.

<details>
    <summary>정답</summary>

```sql
SELCET 기준연도, 학교명, 졸업남자수, 졸업여자수
FROM graduates
WHERE 학교급명 = '특수학교' AND (졸업남자수 + 졸업여자수) >= 25;
```

</details>

---

## 문제 2

2015년에 남, 여 통학 취업률이 50%가 넘은 학교의 지역명, 이름과 취업률(%)를 출력하라.

<details>
    <summary>정답</summary>

```sql
SELECT 학교명, 지역명, 100 * (취업남자수 + 취업여자수)/(졸업남자수 + 졸업여자수) AS 취업률
FROM graduates
WHERE EXTRACT(YEAR FROM 기준연도) = 2015
      AND (졸업남자수 + 졸업여자수) > 0
      AND 100 * (취업남자수 + 취업여자수)/(졸업남자수 + 졸업여자수) >= 50;
```

</details>

---

## 문제 3.

"경기도 고양시 일산" 지역에 있는 고등학교의 각 연도별 졸업생 정보를 다음의 조건을 만족하도록 출력하라.

a. 진학률을 기준으로 내림차순으로 출력하라.

b. 졸업생이 없으면 진학률은 0%로 표시한다.

c. 지역명에 "고양시 일산"이 포함되어 있는 로우를 검색해야 한다.

d. 출력될 때 다음 예시와 같이 기준연도는 연도 숫자로, 지역명에는 "경기도 고양시"라는 문자열을 뺀 뒷부분만 보여야 한다.

<details>
    <summary>정답</summary>

```sql
SELECT EXTRACT(YEAR FROM 기준연도) AS 기준연도, 학교명,
   REPLACE (지역명, '경기도 고양시 ', '') AS 지역명,
   CASE
   WHEN (졸업남자수 + 졸업여자수) = 0
   THEN 0
   ELSE 100 * (진학남자수 + 진학여자수) / (졸업남자수 + 졸업여자수)
   END AS 진학률
FROM graduates
WHERE 지역명 LIKE '경기도 고양시 일산%'
ORDER BY 4 DESC;
```

</details>

---