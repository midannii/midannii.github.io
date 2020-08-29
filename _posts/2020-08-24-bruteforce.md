---
layout: post
title:  "[Python] 백준 알고리즘 완전탐색 : Brute Force) 1476, 9095, 10819, 10971, 1963, 9019, 2251  "
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

- 1
  - (1) -> 1가지

- 2
  - (1+1), (2) -> 2가지

- 3
  - (1+1+1), (2+1), (1+2), (3) -> 4가지

- 4
  - (1+1+1+1), (2+1+1), (1+2+1), (3+1),
  - (1+1+2), (2+2)
  - (1+3) -> 7가지

- 5
  - (1+1+1+1+1), (2+1+1+1), (1+2+1+1), (1+1+2+1), (3+1+1), (1+3+1), (2+2+1),

  - (1+1+1+2), (2+1+2), (1+2+2), (3+2),

  - (1+1+3), (2+3) -> 13가지

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

이건 `list(map(int, input().split()))` 를 찾아보는 과정에서 permutations 에 대한 힌트를 얻은 거임

사실상 나 혼자 푼게 아니지 뭐얌 ㅎㅅㅎ

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



[10971: 외판원문제 2](https://www.acmicpc.net/problem/10971)도 풀었당




```python
def find_cost(lst, w):
    total = 0
    for i in range(len(lst)-1):
        total += w[lst[i]][lst[i+1]]
    return total


n = int(input())
w = []
#  W[i][j]는 도시 i에서 j로 가기 위한 비용
for i in range(n):
    w.append(list(map(int, input().split())))

lst = [i for i in range(0,n)]
from itertools import permutations
lst = permutations(lst)
min_cost = pow(100,10)

for l in lst:
    l = list(l)
    l.append(l[0])
    if min_cost > find_cost(l,w):
        min_cost = find_cost(l,w)

print(min_cost)
```


대강 개념을 이해하니까 나름 쉬웠다고 생각하는데

음 왜 틀렸는지 모르겠당


```python
# 출처: https://hjp845.tistory.com/70

def next_permutation(lst):
    n = len(lst)
    i = n - 1
    while i > 0 and lst[i - 1] >= lst[i]:
        i -= 1
    if i == 0:
        return [-1]
    # i - 1
    j = n - 1
    while lst[j] <= lst[i - 1]:
        j -= 1
    tmp = lst[j]
    lst[j] = lst[i - 1]
    lst[i - 1] = tmp

    lst = lst[:i] + sorted(lst[i:])
    return lst

```

이건 모듈 import가 불가할 때를 대비한 permutation 함수



[이건](https://suri78.tistory.com/152) 외판원 문제를 접근하는 다양한 방법



<br>



[1697: 숨바꼭질](https://www.acmicpc.net/problem/1697)


```python
```



<br>


[1963: ](https://www.acmicpc.net/problem/1963)


```python
```



<br>



[9019: ](https://www.acmicpc.net/problem/9019)

```python
```



<br>




[2251: ](https://www.acmicpc.net/problem/2251)



```python
```



<br>
