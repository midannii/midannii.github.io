---
layout: post
title:  "[Python] 백준 알고리즘 완전탐색 : Brute Force) "
date:   2020-08-03
desc:  " "
keywords: "python, algorithm"
categories: [Python]
tags: [python, algorithim]
icon: icon-html
---




<br>




`brute force algorithm` 이란, 넣을 수 있는 모든 가짓수를 몽땅 다 넣어보는 것이당 !!!

효율이 떨어질 수는 있지만, 답을 구하지 못하는 경우는 거의 없당


<br>





이상한 나라의 준규가 살고 있는 [1476: 날짜 계산](https://www.acmicpc.net/problem/1476) 문제!


```python
e,s,m = map(int, input().split())

for i in range(1, 7981):
    if i % 15 == e or (i % 15 == 0 and e == 15):
        if i % 28 == s or (i % 28 == 0 and s == 28):
            if i % 19 == m or (i % 19 == 0 and m == 19):
                print(i)
```



<br>



```python
```



<br>


```python
```



<br>


```python
```



<br>
