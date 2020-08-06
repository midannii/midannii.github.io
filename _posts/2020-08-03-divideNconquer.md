---
layout: post
title:  "[Python] 백준 알고리즘 분할정복(divide & conquer): 문제풀이  "
date:   2020-08-03
desc:  " "
keywords: "python, algorithm, tree"
categories: [Python]
tags: [python, algorithim]
icon: icon-html
---



`divide & conquer` 즉 분할 정복은

전체를 부분으로 나누어 부분의 문제를 해결함으로써  전체의 문제를 해결하는 PS 방법이다.

대표적인 예로 `binary search`나  `merge sort`, `quick sort` 등이 있당


<br>

[11728: 배열 합치기](https://www.acmicpc.net/problem/11728)



```python
n, m= map(int, input().split())
a = [i for i in map(int,input().split())]
b = [i for i in map(int,input().split())]
result = a+b
result.sort()
for re in result:
    print(re, end = ' ')
```

어쩌면 python이라 너무 쉬웠던 문제,,,

분할정복을 잘 썼는지는 모르겠당




<br>


[1780: 종이자르기](https://www.acmicpc.net/problem/1780)


```python
n = int(input())
paper = [[] for i in range(n)]
for i in range(n):
    paper[i] = [i for i in map(int, input().split())]

def check_paper(p):
    standard = p[0][0]
    for i in range(len(p)):
        for j in range(len(p)):
            if p[i][j]!=standard:
                return False
    return True    

def divide_paper(p):
    result = [[] for i in range(9)]
    for i in range(9):
         for j in range(9):   
            if i <3:
                if j<3: result[0].append(p[i][j])
                elif j<6: result[1].append(p[i][j])
                else: result[2].append(p[i][j])
            elif i<6:
                if j<3: result[3].append(p[i][j])
                elif j<6: result[4].append(p[i][j])
                else: result[5].append(p[i][j])
            else:
                if j<3: result[6].append(p[i][j])
                elif j<6: result[7].append(p[i][j])
                else: result[8].append(p[i][j])

    for i in range(9):
        result[i] = make_square(result[i])

    return result

def make_square(lst):
    n = int(len(lst)/3)
    result = [[] for i in range(n)]
    j = 0
    for i in range(len(lst)):
        result[j].append(lst[i])
        if (i+1)%3==0: j+=1
    return result    

def solve(p, counts):
    if len(p)==1 or check_paper(p): # base case 1
        if p[0]==-1: counts[0]+=1
        elif p[0]==0: counts[1]+=1
        else: counts[2]+=1
        return counts    

    else: #not check_paper(p)
        if len(p)==4: # base case 2
            for i in range(len(p)):
                if p[i]==-1: counts[0]+=1
                elif p[i]==0: counts[1]+=1
                else: counts[2]+=1
            return counts

        elif len(p)>4: # divide
            new = divide_paper(p)
            for n in new:
                return solve(n, counts)
counts = [0,0,0]    
for i in solve(paper, counts):
    print(i)   

```

뭔가 열심히 짜긴 했는데 음

마지막 `solve` 부분에서 분할정복을 조금 더 녹였어야 하나 싶음,,,

그래서 [다른 분들의 코드](https://developmentdiary.tistory.com/336)를 조금 살폈당


 함수로 모듈화한 부분들을 줄이고, 되도록 for문을 적게 사용하고자 해야하구나,,,



<br>

[11729: 하노이탑](https://www.acmicpc.net/problem/11729) 은 어엄청 유명한 문제,,

이게 난 완탐이나 dp일 줄 알았는데 분할정복이었구만



```python
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
