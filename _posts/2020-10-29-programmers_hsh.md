---
layout: post
title:  "[Programmers] 알고리즘 오랜만에 복습 [6] heap, stack, queue, hash - 더 맵게, 주식 가격, 다리를 지나는 트럭, 전화번호 목록 "
date:   2020-10-29
desc: " "
keywords: "python, Programmers, algorithm"
categories: [Python]
tags: [python, algorithm, heap, stack, queue, hash]
icon: icon-html
---
<br>


1. 더 맵게 [heap]

이렇게 하면 test case는 통과하지만, 효율성 테스트에서는 시간 초과,,, 결과는 대부분 런타임 에러,,,

힙을 안쓴 코드여서 그렇지 뭐

```python
def solution(scoville, K):
    '''
    input: Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville, 원하는 스코빌 지수 K
    task: 섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
    output: 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수
    모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return

    '''
    answer = 0
    for i in range(10**10):
        scoville.sort()
        answer +=1
        scoville.append(scoville[0]+scoville[1])
        del scoville[0]
        del scoville[0]

        if scoville[0] >=K: return answer
    return -1
```

사실 min이 맨 위에 있는 minheap 구조를 만들면 좋겠지만, 계속 정렬하는 과정에서 오래걸릴 것 같당..

but, python에는 `heapq` 라는 라이브러리가 있었고..! 이를 이용해보자


```python
import heapq
def solution(scoville, k):
    heap = []
    for num in scoville:
        heapq.heappush(heap, num)
        cnt = 0
        print(heap)
    while heap[0] < k:
        print(heap)
        try:
             heapq.heappush(heap, heapq.heappop(heap) + (heapq.heappop(heap) * 2))
        except IndexError:
            return -1
        cnt += 1

    return cnt
```


2. 주식 가격 [queue]

문제 이해하는데 쫌 걸렸다,,

```python
from collections import deque
def solution(prices):
    '''
    초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때,
    가격이 떨어지지 않은 기간은 몇 초인지를 return
    '''
    answer = []
    prices = deque(prices)

    while prices:
        temp = prices.popleft() # 기준
        count = 0
        for price in prices:  # 나머지 숫자 하나씩 다 살펴보기
            count += 1
            if temp > price: break
        answer.append(count)
    return answer
```


3. 다리를 지나는 트럭 [stack]

```python
```


4. 전화번호 목록 [hash]

```python
```
