---
layout: post
title:  "[Programmers] SQL 알고리즘 [2] Sum min max - 최댓값 구하기, 최솟값 구하기, 동물 수 구하기  , 중복 제거  "
date:   2020-10-29
desc: " "
keywords: "python, Programmers, algorithm"
categories: [Database]
tags: [sql, algorithm, db]
icon: icon-html
---


1. 최댓값 구하기

```sql
SELECT MAX(DATETIME) from ANIMAL_INS;
```

2. 최솟값 구하기

```sql
SELECT MIN(DATETIME) from ANIMAL_INS;
```

3. 동물 수 구하기

```sql
SELECT COUNT(ANIMAL_ID) from ANIMAL_INS;
```

4. 중복 제거

```sql
SELECT COUNT(DISTINCT NAME) from ANIMAL_INS
WHERE NAME is NOT NULL;
```
