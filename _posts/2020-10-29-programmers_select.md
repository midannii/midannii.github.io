---
layout: post
title:  "[Programmers] SQL 알고리즘 [1] SQL -  모든 레코드 조회하기 , 아픈 동물 찾기, 상위 n개 레코드  "
date:   2020-10-29
desc: " "
keywords: "python, Programmers, algorithm"
categories: [DB&BE]
tags: [sql, algorithm, db]
icon: icon-html
---


1. 모든 레코드 조회하기

```sql
SELECT ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE
from ANIMAL_INS
order by ANIMAL_ID;
```


2. 아픈 동물 찾기

```sql
SELECT ANIMAL_ID, NAME from ANIMAL_INS
where INTAKE_CONDITION = 'Sick';
```


3. 상위 N개 레코드

```sql
SELECT NAME from ANIMAL_INS
order by DATETIME limit 1;
```
