---
layout: post
title:  "[Programmers] 알고리즘 오랜만에 복습 [2] Graph - 가장 먼 노드 "
date:   2020-10-26
desc: " "
keywords: "python, Programmers, algorithm"
categories: [Python]
tags: [python, algorithm, BinarySearch]
icon: icon-html
---
<br>

아예 그래프 유형을 중점적으로 하는 문제를 다뤄본건 처음이야,,

<br>


## 가장 먼 노드

처음에 작성한 코드는 아래와 같다.

```python
def solution(n, edge):
    '''
    input: 노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex
    output: 1번 노드로부터 가장 멀리 떨어진 노드(최단경로로 이동했을 때 간선의 개수가 가장 많은 노드)가 몇 개인지
    각 노드는 1부터 n까지 번호가 적혀있음
    '''
    answer = 0
    # 각 node가 어떤 node와 연결되어 있는지 정리
    tree = [[] for i in range(n)]
    for i in range(1,n+1):
        for e in edge:
            e.sort()
            if i in e: tree[i-1].append(e)
    max_count = 0
    for t in tree[0]: # 1번째 node에 대해,
        temp_count = 1
        dest_node = t[1]
        steps = [1]
        while True:
            new = tree[dest_node-1]
            steps.append(dest_node)
            temp_count += 1
            # 종료조건 1: 모든 탐색이 끝난 경우
            if len(steps) == n: break
            # 종료조건 2: 마지막 node인 경우
            if len(new)==1: break
            # 다음 node로 넘어가기
            for i in range(2, n+1):
                if i not in steps:
                    dest_node = i
        max_count = max(max_count, temp_count)

    answer = max_count
    return answer
```


답이 틀린 것도 있지만, 시간초과도 네가지 case나 있어서 수정이 필요했음 !

결국은 그냥 BFS 아니면 DFS로 풀기로 결심했다!


그런데 대충 다른 분들의 코드를 보니 다들 BFS로 푸셨더라,,

아마도 1에서 그 다음 node인 2,3을 모두 시도해야 하기 때문이 아니였나 싶다!

하여튼 BFS 를 이용해서 풀어본 코드는 아래와 같다

```python

import queue
# with BFS
def solution(n, edge):
    '''
    input: 노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex
    output: 1번 노드로부터 가장 멀리 떨어진 노드(최단경로로 이동했을 때 간선의 개수가 가장 많은 노드)가 몇 개인지
    각 노드는 1부터 n까지 번호가 적혀있음
    '''
    answer = 0
    # 각 node가 어떤 node와 연결되어 있는지 정리
    nodeList = []
    adjencyList = [[] for i in range(n+1)]
    for e in edge: #각각 1~n이 어떤 vertex와 edge를 가지고 있는지
        adjencyList[e[0]].append(e[1])
    #print(adjencyList)
    q=queue.Queue()
    q.put(1)
    visitedVertex = []
    while q.qsize():
        current = q.get()
        for neighbor in adjencyList[current]: # 해당 node에 연결되어 있는 애들 파악하기
            if not neighbor in visitedVertex:
                visitedVertex.append(neighbor)
                if len(adjencyList[neighbor])==1: break
                else:
                    for nei in adjencyList[neighbor]:
                        q.put(nei)
            #
            #    q.put(neighbor)
        #visitedVertex.append(current)

    answer = len(visitedVertex)
    return answer
```

그치만 역시 이번에도 일부만 맞았구,,

일단 오늘은 여기까지 하구, BFS 와 DFS를 풀어본 이후에 다시 도전해봐야겠당
