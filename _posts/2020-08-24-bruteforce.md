---
layout: post
title:  "[Python] 백준 알고리즘 완전탐색 : Brute Force) 1476, 9095, 10819, 10971, 1697, 1963, 9019, 2251  "
date:   2020-08-03
categories: algorithm
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



[1697: 숨바꼭질](https://www.acmicpc.net/problem/1697)도 DP로 풀려다가

아 이건 아닌거 같아서 구글링해보니 BFS 로 푸는 경우도 많더랑,,,


```python

count = 0
n, k = map(int, input().split()) # 각각 수빈, 동생의 위치
print(solve(n,k))

# https://chaewonkong.github.io/posts/python-boj-1697.html
from collections import deque

MAX = 100001
def solution(n, k):
    q = deque([n])
    visit = [0] * MAX

    def nextPos(next, cur):
        if 0 <= next and next < MAX:
            if visit[next] == 0 or (visit[cur] + 1 < visit[next]):
                visit[next] = visit[cur] + 1
                q.append(next)

    while q:
        cur = q.popleft()
        if cur == k:
            return visit[cur]
        nextPos(cur - 1, cur)
        nextPos(cur + 1, cur)
        nextPos(cur * 2, cur)

```



<br>


[1963: 소수경로](https://www.acmicpc.net/problem/1963)는

단순 재귀로 풀고 싶어서 아래처럼 풀었는데 다들 BFS로 푸시더라구,,,


```python
# 에라토스테네스의 채 이용
num=10000
a = [False,False] + [True]*(n-1)
primes=[]

for i in range(1000,num+1):
    if a[i]:
        primes.append(i)
        for j in range(2*i, n+1, i):
            a[j] = False

def solve(a, b):
    if a!=b:
        a_num = [int(str(a)[0]), int(str(a)[1]) , int(str(a)[2]) , int(str(a)[3]) ]
        b_num = [int(str(b)[0]), int(str(b)[1]) , int(str(b)[2]) , int(str(b)[3]) ]
        # case 1: 천의 자리
        if (a_num[0] > b_num[0]) and (a-1000 in primes):
            return 1+solve(a-1000, b)
        elif (a_num[0] < b_num[0]) and (a+1000 in primes):
            return 1+solve(a+1000, b)
        # case 2: 백의 자리
        if (a_num[1] > b_num[1]) and (a-100 in primes):
            return 1+solve(a-100, b)
        elif (a_num[0] < b_num[0]) and (a+100 in primes):
            return 1+solve(a+100, b)
        # case 3: 십의 자리
        if (a_num[2] > b_num[2]) and (a-10 in primes):
            return 1+solve(a-10, b)
        elif (a_num[2] < b_num[2]) and (a+10 in primes):
            return 1+solve(a+10, b)
        # case 4: 일의 자리
        if (a_num[3] > b_num[3]) and (a-1 in primes):
            return 1+solve(a-1, b0)
        elif (a_num[3] < b_num[3]) and (a+1 in primes):
            return 1+solve(a+1, b)
    else:
        return 0

n = int(input())    
result = []
for i in range(n):
    a,b = map(int,input().split())
    result.append(solve(a, b))

for re in result:
    print(re)
```



<br>



[9019: DSLR ](https://www.acmicpc.net/problem/9019) 은 좀 어려워서 , 구글링했당

생각보다 BFS, DFS 로 접근하는 경우가 많아서, 개강하고 알고리즘 수업 들으면 다시 풀어봐야징

```python
# https://suri78.tistory.com/155

import sys
from collections import deque

def bfs():
    queue = deque([(first, '')])
    visited[first] = 1

    while queue:
        now, cmd = queue.popleft()
        if now == last:
            return cmd

        l_now = len(str(now))

        t = (now*2)%bound
        if not visited[t]:
            visited[t] = 1
            queue.append((t, cmd + 'D'))

        t = (now-1)%bound
        if not visited[t]:
            visited[t] = 1
            queue.append((t, cmd + 'S'))

        if l_now != 4:
            t = now*10
        else:
            t, d = divmod(now, 10**(l_now-1))
            t += (d*10)
        if not visited[t]:
            visited[t] = 1
            queue.append((t, cmd + 'L'))

        if l_now == 1:
            t = now*1000
        else:
            t, d = divmod(now, 10)
            t += (d*1000)
        if not visited[t]:
            visited[t] = 1
            queue.append((t, cmd + 'R'))


testcase = int(sys.stdin.readline())
bound = 10000
for _ in range(testcase):
    first, last = map(int, sys.stdin.readline().split())
    visited = [0]*bound
    print(bfs())


출처: https://suri78.tistory.com/155 [공부노트]

```
<br>
