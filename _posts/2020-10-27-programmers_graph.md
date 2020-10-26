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

```Python
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

결국은 그냥 BFS, 아니면 DFS로 풀기로 결심했다!


```Python
```


<br>


```Python
```

<br>
