---
layout: post
title:  "[Python] 백준 알고리즘 이진탐색 알고리즘(binary search): 1654, 2805, 2110, 10815, 10816, 11662 "
date:   2020-08-31
desc:  " "
keywords: "python, algorithm"
categories: [Python]
tags: [python, algorithim]
icon: icon-html
---




<br>



binary search는, 오름차순으로 정렬된 리스트에서 특정한 값의 위치를 찾는 알고리즘이다.

처음 중간의 값을 임의의 값으로 선택하여, 그 값과 찾고자 하는 값의 크고 작음을 비교하는 방식을 채택한당



<br>

[1654: 랜선 자르기](https://www.acmicpc.net/problem/1654) 는 대표적인 bs 문제당.

처음엔 이게 왜 ..? 싶었는데 찾아보니 찾아야 할 값은 `랜선의 길이`, 이분탐색의 기준이 `랜선의 개수` 이당


```python
k, n = map(int, input().split())

lst = []
for i in range(k):
    lst.append(int(input()))

low, high = 1, max(lst)
mid = (low+high)//2

total = 0
while(True):
    total = 0
    for l in lst:
        total += l//mid
    if total >n:
        low = mid
        mid = (mid+high)//2
    elif total < n:
        high = mid
        mid = (low+mid)//2
    else: # total == n
        break
while(total >= n):
    mid +=1
    total = 0
    for l in lst:
        total += l//mid
print(mid-1)
```

이렇게 하면 시간초과 두둥


```python
k, n = map(int, input().split())

lst = []
for i in range(k):
    lst.append(int(input()))

low, high = 1, max(lst)
mid = (low+high)//2

total = 0
while(not ifFound):
    total = 0
    for l in lst:
        total += l//mid
    if total >n:
        low = mid
        mid = (mid+high)//2
    elif total < n:
        high = mid
        mid = (low+mid)//2
    else: # total == n
        max_len = high
        while(True):
            total = 0
            for l in lst:
                total += l//max_len
            if total == n:
                ifFound = True
                break
            else:
                max_len -=1
print(max_len)
```


<br>

[2805: 나무 자르기](https://www.acmicpc.net/problem/2805)는


```python
n, m = map(int, input().split())
a = list(map(int, input().split()))

high, low = 1, max(a)
total = 0
while (total !=m):
    length = (high+low)//2
    total = 0
    for i in a:
        if (i-length)>0:
            total += (i-length)
    if total > m:
        high = length
    elif total < m:
        low = length
print(length)
```

이랬더니 시간 초과가 떠서 ㅠㅠ

아래와 같이 조금 수정했당


```python
n, m = map(int, input().split())
a = list(map(int, input().split()))

high, low = 1, max(a)
total = 0
while (low <= high):
    length = (high+low)//2
    total = 0
    for i in a:
        if (i-length)>0:
            total += (i-length)
    if total >= m:
        high = length+1
    elif total < m:
        low = length-1
print(length)
```



<br>


[2110: 공유기 설치 ](https://www.acmicpc.net/problem/2110)



```python
```



<br>

[10815: 숫자 카드](https://www.acmicpc.net/problem/10815)


```python
```






<br>


[11662: 민호와 강호 ](https://www.acmicpc.net/problem/11662) 는 binary search로 적용하는 아이디어가 너모 힘들었당

알고보니까 tetra search 였음 엉엉 



```python
```
