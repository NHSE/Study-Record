
# 📘 Problem 1

## 문제 1. 데이터 베이스 만들기

커뮤니티에 있는 게시판을 사용할 수 있도록 구조를 구축할 것이다. 먼저 다음 이름을 지닌 새로운 데이터 베이스를 생성한다.

- 데이터 베이스명 : `community_board`

<details>
    <summary>정답</summary>

```sql
CREATE DATABASE community_board;
```

</details>

---

## 문제 2. 유저 테이블 만들기

익명 게시판이 아닌 직접 사용자가 누구인지 정보 조회가 되는 커뮤니티를 만들 것이다. 따라서 우리는 유저 테이블을 만든다. 다음과 같이 테이블 명과 컬럼 속성을 갖도록 새로운 테이블을 생성하자.
이때 user_pk는 프라이머리 키로 생성하라

- 테이블명 : users
    - 컬럼명 : user_pk[INTEGER] -> R
    - 컬럼명 : user_id[VARCHAR(80)]
    - 컬럼명 : user_pw[VARCHAR(12)]
    - 컬럼명 : register_date[DATE]

<details>
    <summary>정답</summary>

```sql
CREATE TABLE users ( user_pk INTEGER, user_id VARCHAR(80), user_pw VARCHAR(12), register_date DATE, PRIMARY KEY (user_pk));
```

</details>

---

## 문제 3. 게시판 테이블 만들기

어느 유저로 인해 게시글이 생성되면 이에 대한 정보를 다루는 테이블도 필요하다. 따라서 우리는 게시판 테이블을 만든다. 다음과 같이 테이블명과 컬럼 속성을 갖도록 새로운 테이블을 생성하자.

- 테이블명 : board
    - 컬럼명 : board_pk[INTEGER] -> R
    - 컬럼명 : board_user[VARCHAR(80)]
    - 컬럼명 : register_date[DATE]
    - 컬럼명 : title[VARCHAR(30)]
    - 컬럼명 : description[VARCHAR(3000)]
    - 컬럼명 : likes[INTEGER]
    - 컬럼명 : image_name[VARCHAR(50)]

<details>
    <summary>정답</summary>

```sql
CREATE TABLE board ( 
board_pk INTEGER, board_user VARCHAR(80), register_date DATE, title VARCHAR(30), description VARCHAR(3000), likes INTEGER, image_name VARCHAR(50)
);
```

</details>

---

## 문제 4. 다음 데이터를 직접 유저와 게시판 테이블에 넣기

다음은 예시 유저 데이터이다. 각 컬럼에 맞게 테이터를 데이터베이스에 삽입하자.
||||
|---------|---------|--------|
|Carveinus|car1234|2020/04/23|
|Jenna|kk3375|2020/07/12|
|Wlfur|fur0022|2020/08/31|

다음은 게시글 데이터이다. 각 컬럼에 맞게 데이터를 데이터베이스에 삽입하자.
||||||
|-|-|-|-|-|
|Developer's essay| Perhaps the reason we develop is because of the sense of accomplishment when we create something useful |  | 2020/05/02 | 1 |
|Why the earth is round| I took a picture myself from space and saw that the earth is round | er.png | 2020/09/28 | 3 |
|Coffee time| I had a vanilla latte this afternoon at the blue signboard cafe on the boulevard. | coffee.jpeg | 2020/07/13 | 2 |
|Chicken is inefficient| This is because fried chicken is more expensive than other chicken dishes. | | 2020/08/14 | 2 |
|When bothering| Let's get someone else to work | | 2020/06/22 | 1 |

<details>
    <summary>정답</summary>

```sql
INSERT INTO users (user_id, user_pw, register_date) VALUES
('Carveinus', 'car1234', '2020/04/23'),
('Jenna', 'kk3375', '2020/07/12'),
('Wlfur', 'fur0022', '2020/08/31');

INSERT INTO board (title, description, image_name, register_date, board_user) VALUES
('Developer''s essay', 'Perhaps the reason we develop is because of the sense of accomplishment when we create something useful', '', '2020/05/02', 1),
('Why the earth is round', 'I took a picture myself from space and saw that the earth is round', 'er.eng', '2020/09/28', 3),
('Coffee time', 'I had a vanilla latte this afternoon at the blue signboard cafe on the boulevard.', 'coffee.jpeg', '2020/07/13', 2),
('Chicken is inefficient', 'This is because fried chicken is more expensive than other chicken dishes.', '', '2020/09/28', 2),
('When bothering', 'Let''s get someone else to work', '', '2020/06/22', 1);

```

</details>

---

## 문제 5. 한 페이지에 등록날짜를 desc 정렬하여 최근 3개 게시글 조회하기
게시글 제목과 내용만 표시하자.

<details>
    <summary>정답</summary>

```sql
SELECT title, description 
FROM board 
ORDER BY register_date DESC 
LIMIT 3;
```

</details>

---

## 문제 6. 유저의 비밀번호 변경하기
Carveinus 유저의 비밀번호를 car4321로 변경하자.

<details>
    <summary>정답</summary>

```sql
UPDATE users
SET user_pw = 'car4321'
WHERE user_id = 'Carveinus'
```

</details>

---

## 문제 7. 불쾌한 내용이 있는 게시판 삭제
When bothering 게시글이 불쾌하다며 신고되었다고 가정한다. 그 글을 삭제하자.

<details>
    <summary>정답</summary>

```sql
DELETE FROM board WHERE title = 'When bothering'
```

</details>

---