---
layout: post
title:  "[Programmers] SQL 알고리즘 [4] JOIN  -  없어진 기록 찾기, 있었는데요 없었습니다, 오랜 기간 보호한 동물, 보호소에서 중성화한 동물  "
date:   2020-10-30
categories: algorithm
---

1. 없어진 기록 찾기

```sql
SELECT A.ANIMAL_ID, A.NAME
FROM ANIMAL_OUTS A LEFT JOIN ANIMAL_INS B
ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE B.ANIMAL_ID IS NULL
ORDER BY A.ANIMAL_ID
```

2. 있었는데요 없었습니다

```sql
SELECT A.ANIMAL_ID, B.NAME from ANIMAL_INS A
LEFT JOIN ANIMAL_OUTS B on A.ANIMAL_ID = B.ANIMAL_ID
WHERE (A.DATETIME - B.DATETIME) > 0
order by A.DATETIME;
```

3. 오랜 기간 보호한 동물(1)

```sql
SELECT A.NAME, A.DATETIME from ANIMAL_INS A
LEFT JOIN ANIMAL_OUTS B
ON A.ANIMAL_ID = B.ANIMAL_ID
where B.ANIMAL_ID is null
order by A.DATETIME
limit 3;
```

4. 보호소에서 중성화한 동물

```sql
SELECT A.ANIMAL_ID, A.ANIMAL_TYPE, A.NAME from ANIMAL_INS A
RIGHT JOIN ANIMAL_OUTS B on B.ANIMAL_ID = A.ANIMAL_ID
 WHERE A.SEX_UPON_INTAKE like '%Intact%' and
     (B.SEX_UPON_OUTCOME like '%Spayed%' or B.SEX_UPON_OUTCOME like '%Neutered%')
```
