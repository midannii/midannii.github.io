---
layout: post
title:  "[Programmers] SQL 알고리즘 [3] Group by & is null  -  고양이와 개는 몇마리 있을까, 입양시각 구하기, 이름이 있는/없는 동물의 id  "
date:   2020-10-30
desc: " "
keywords: "python, Programmers, algorithm"
categories: [Database]
tags: [sql, algorithm, db]
icon: icon-html
---

1. 고양이와 개는 몇마리 있을까


```sql
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) from ANIMAL_INS
GROUP BY ANIMAL_TYPE
order by ANIMAL_TYPE;
```

2. 입양시각 구하기

  (1)
```sql
SELECT HOUR(DATETIME) HOUR, COUNT(HOUR(DATETIME)) COUNT from ANIMAL_OUTS
where HOUR(DATETIME)>8 and HOUR(DATETIME)<20
group by HOUR(DATETIME)
order by HOUR(DATETIME);
```

  (2)

```sql
-- https://qastack.kr/dba/174694/how-to-get-a-group-where-the-count-is-zero
CREATE TABLE hours (
  HOURS int primary key
);

insert into streets values (0), (1), (2), (3), (4), (5),
                           (18), (19), (20), (21), (22), (23);

SELECT HOUR(DATETIME) HOUR, COUNT(HOUR(DATETIME)) COUNT from ANIMAL_OUTS
where HOUR(DATETIME)>=0 and HOUR(DATETIME)<24
LEFT OUTER JOIN hours ON hours.HOURS = ANIMAL_OUTS.HOUR(DATETIME )
group by HOUR(DATETIME)
order by HOUR(DATETIME);
```


이렇게 하려고 했지만,, create는 불가능했다 ㅠㅠ

```sql
--https://chanhuiseok.github.io/posts/db-6/
SET @hour := -1; -- 변수 선언

SELECT (@hour := @hour + 1) as HOUR,
(SELECT COUNT(*) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @hour) as COUNT
FROM ANIMAL_OUTS
WHERE @hour < 23

```

3. 이름이 있는 동물의 id

```sql
SELECT ANIMAL_ID from ANIMAL_INS
where NAME is not null;
```
4. 이름이 없는 동물의 id

```sql
SELECT ANIMAL_ID from ANIMAL_INS
where NAME is null;
```
