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



[9095: 1,2,3으로 나타내기](https://www.acmicpc.net/problem/9095) 는  

뭔가 약간 DP스러워서 엥?? 이런 느낌이었당


왜냐면,

1 = (1) -> 1가지

2 = (1+1), (2) -> 2가지

3 = (1+1+1), (2+1), (1+2), (3) -> 4가지

4 = (1+1+1+1), (2+1+1), (1+2+1), (1+1+2), (3+1), (1+3), (2+2) -> 7가지

5 = (1+1+1+1+1), (2+1+1+1), (1+2+1+1), (1+1+2+1), (3+1+1), (1+3+1), (2+2+1),

    (1+1+1+2), (2+1+2), (1+2+2), (3+2),

    (1+1+3), (2+3) -> 13가지

뭐 이런 식이었기 때문,, 즉 정리하자면


`n번째 숫자의 경우의 수`는 `n-1에 1을 더한 경우의 수` + `n-2에 2를 더한 경우의 수` + `n-3에 3을 더한 경우의 수` 인 것이당 !



```python
def solve(num, lst):
    if lst[num] == 0:
        lst[num] = solve(num-1, lst) + solve(num-2, lst) + solve(num-3, lst)
    return lst[num]

n = int(input())
nums = []
lst = [0,1,2,4] +[0]*7 #[0, 1, 2, 4, 0, 0, 0, 0, 0, 0, 0]
for i in range(n):
    num = int(input())
    nums.append(solve(num, lst))

for num in nums:
    print(num)
```



<br>

[10819: 차이를 최대로](https://www.acmicpc.net/problem/10819)

이건 

```python
def check_list(lst):
    result = 0
    for i in range(1,len(lst)):
        result += abs(lst[i]-lst[i-1])
    return result

n = int(input())
num_list = list(map(int, input().split()))

from itertools import permutations
total = 0
for lst in permutations(num_list):
    if total < check_list(lst):
        #print(check_list(lst))
        total = check_list(lst)
print(total)
```



<br>


```python
```



<br>
