---
layout: post
title:  "[Programmers] SQL 알고리즘 [4] string & date  - 이름에 el이 들어가는 동물 찾기, 오랜 기간 보호한 동물, DATETIME에서 DATE로 형 변환   "
date:   2020-10-30
desc: " "
keywords: "python, Programmers, algorithm"
categories: [Database]
tags: [sql, algorithm, db]
icon: icon-html
---


1. 이름에 el이 들어가는 동물 찾기


```sql
SELECT ANIMAL_ID, NAME from ANIMAL_INS
where NAME LIKE '%el%' and ANIMAL_TYPE = 'Dog'
order by NAME;
```

2. 오랜 기간 보호한 동물(2)

어려웠던 문제 ㅜㅜ inner join으로 풀려고 했지만 ,, 찾아보니 아니였당


```sql
-- https://aorica.tistory.com/88
SELECT A.ANIMAL_ID, A.NAME
FROM ANIMAL_INS A, ANIMAL_OUTS B
WHERE A.ANIMAL_ID = B.ANIMAL_ID
ORDER BY B.DATETIME-A.DATETIME DESC
LIMIT 2;
```

3. DATETIME에서 DATE로 형 변환

```sql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') AS 날짜
 from ANIMAL_INS;
```
