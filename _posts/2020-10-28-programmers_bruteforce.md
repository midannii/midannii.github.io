---
layout: post
title:  "[Programmers] 알고리즘 오랜만에 복습 [4] 완전탐색 & greedy - 카펫, 섬 연결하기  "
date:   2020-10-28
desc: " "
keywords: "python, Programmers, algorithm"
categories: [Python]
tags: [python, algorithm, BruteForce]
icon: icon-html
---
<br>


1. 카펫 [완전탐색 ]

이 문제에서, 가로 x 세로 y라고 하면, brown = 2(x-2+y-2)+4, yellow = (x-2)(y-2)= xy-x-y+4 이며 brown+yellow = xy이다.


```python
def solution(brown, yellow):
    '''
    중앙에는 노란색으로 칠해져 있고 테두리 1줄은 갈색으로 칠해져 있는 격자 모양 카펫
    input: Leo가 본 카펫에서 갈색 격자의 수 brown, 노란색 격자의 수 yellow
    output: 카펫의 가로, 세로 크기를 순서대로 담은 배열
    '''
    answer = []
    # x+y = -3, 즉 y = -3-x
    # xy = brown+yellow

    # 가로가 제일 작은 경우 = yellow의 가로가 제일 작은 경우: x = 1+2, y = yellow+2
    # 가로가 제일 큰 경우 : x = yellow+2, y = 1+2

    for i in range(3,yellow+3): # x의 값의 변화
        y = (brown+yellow)/i
        if i>=y and int(y) == y and brown == (i-2+y-2)*2+4:
            answer=[i,int(y)]
            break
    return answer
```





2. 섬 연결하기  [greedy]

맨 처음에는 부분의 최소가 전체의 최소가 되지 않을까? 생각하고 아래와 같이 풀었당

```python
def solution(n, costs):
    '''
    input: n개의 섬 사이에 다리를 건설하는 비용(costs)
    output: 최소의 비용으로 모든 섬이 서로 통행 가능하도록 만들 때 필요한 최소 비용

    condition: 다리를 여러 번 건너더라도, 도달할 수만 있으면 통행 가능하다
    임의의 i에 대해, costs[i][0] 와 costs[i] [1]에는 다리가 연결되는 두 섬의 번호가 들어있고,
    costs[i] [2]에는 이 두 섬을 연결하는 다리를 건설할 때 드는 비용
    '''
    answer = 0
    curr = 0
    visited = [0]*n
    while sum(visited)!=n: # 매순간, 최소비용인 것들 택해서 더하기
        temp_ind = []
        visited[curr]=1
        #print(curr)
        min_ = 1000
        ind = 0
        for i in range(n):
            if costs[i][0]==curr or costs[i][1]==curr:
                if min_> costs[i][2]:
                    min_ = costs[i][2]
                    #print(min_)
                    ind = i
            answer += min_
        if costs[ind][0] == curr: curr = costs[ind][1]
        else: curr = costs[ind][0]
        if visited[curr]==1: break
        #print()
    return answer
```

왜 처음에 그렇게 생각했는지 모르겠닼ㅋㅋ쿠 local 한 최적만을 수집한다고 해서 최종 해답이 꼭 최적인 건 아니니,,,


찾아보니, 이는 `Kruskal 알고리즘` 을 이용한 문제라고 한다!


```
// 출처: https://ysyblog.tistory.com/33

▶ Kruskal 알고리즘 이란?

- 탐욕적인 방법(greedy method) 을 이용하여 가중치를 간선에 할당한 그래프(네트워크)의 모든 정점을 최소 비용으로 연결하는 최적 해답을 구하는 것

- 탐욕적인 방법이란?
결정 할 때마다 그 순간에 가장 좋다고 생각되는 것을 선택하며 최적의 해답에 도달하는 것
탐욕적인 방법은 그 순간에는 최적이지만, 전체적인 관점에서 최적이라는 보장이 없기 때문에 반드시 검증해야 한다.

- Kruskal 알고리즘은 최적의 해답을 주는 것으로 증명되어 있으며 아래 두 조건을 만족해야한다

1) 최소 비용 신장 트리(MST)가 최소 비용의 간선으로 구성되었는가
2) 사이클을 포함하지 않음 의 조건에 근거하여 각 단계에서 사이클을 이루지 않는 최소 비용 간선을 선택 한다.

▶ Kruskal 알고리즘의 구현

1) 그래프의 간선들을 가중치를 기준으로 오름차순으로 정렬한다.
2) 정렬된 리스트에서 사이클이 발생하지 않도록 간선을 선택한다.
- 즉, 가장 낮은 가중치를 먼저 선택함과 동시에 사이클을 형성하는 간선을 제외한다.
3) 해당 간선을 현재의 최소 비용 신장 트리(MST)의 집합에 추가한다.

```

나한테는 너무 어려운 greedy ㅜㅜ 그래서 [참고](https://ysyblog.tistory.com/33)의 코드를 이해하는 데에 초점을 두었다!

```python
# https://ysyblog.tistory.com/33
def solution(n, costs):
    answer = 0
    bridge = 0
    cycle = {k:k for k in range(n)}  #cycle 도는것을 방지하기 위해 만듦
    costs.sort(key=lambda x : x[2]) #cost를 오름차순으로 정렬
    for i in costs:
        if i[0] > i[1]: #i[0]이 #i[1]보다 큰 경우 바꿔줌
            i[0], i[1] = i[1], i[0]
        if cycle[i[1]] == cycle[i[0]]: #cycle의 value가 같으면 이미 연결되었다는 것임
            continue
        answer += i[2]
        bridge += 1
        past = cycle[i[1]] #i[1]을 i[0]의 value값으로 바꾸기 전에 저장 -> i[1]과 같은 value값을 가지고 있는 것도 바꿔줘야하기 때문
        cycle[i[1]] = cycle[i[0]] #나보다 작은 숫자의 섬으로 cycle의 value를 바꿔줌
        for k, v in cycle.items(): #만약 0-1-2, 3-4처럼 고립된상태에서 2-3이 연결되면 4의 value도 바꿔주어야함
            if v==past:
                cycle[k]=cycle[i[0]]
        if bridge == n-1:
            break
    return answer



```
