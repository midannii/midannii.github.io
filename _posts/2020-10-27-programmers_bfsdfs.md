---
layout: post
title:  "[Programmers] 알고리즘 오랜만에 복습 [3] BFS,DFS - 타켓 넘버, 네트워크 "
date:   2020-10-27
desc: " "
keywords: "python, Programmers, algorithm"
categories: [Python]
tags: [python, algorithm, dfs, bfs]
icon: icon-html
---
<br>


1. 타겟 넘버


절대값이 주어진 숫자인 음, 양의 두가지 숫자를 node로 하는 트리를 생각해볼 수 있다.

즉, `2^len(number)`개의 node를 갖는 트리가 생기는 셈!

이때 DFS를 통해, 각 path별로 지나온 길들을 다 더한 총합이 target이 되는 경우의 수를 생각해보면 된다.


```python
numbers = [1, 2, 3, 4]
result = []
for i in range(len(numbers)-1):
    result.append([numbers[i], [numbers[i+1], -numbers[i+1]]])
    result.append([-numbers[i], [numbers[i+1], -numbers[i+1]]])
```

처음에는 이렇게, 해당 node의 다음에는 어떤 Node가 존재할 수 있는지를 담은 result를 만들어 접근하려 했다.


```
[[1, [2, -2]],
 [-1, [2, -2]],
 [2, [3, -3]],
 [-2, [3, -3]],
 [3, [4, -4]],
 [-3, [4, -4]]]
```


이건 약간 dfs, bfs 의 정석??

그런데 이게 진짜 ㅋㅋㅋ 저번에 DFS 풀었을 떄에도 똑같이 열심히 vertex array 만들었는데,,,

다들 dfs 함수 만들어서 쓰는 거 보고 충격이었던 내 모습이 떠올라 급하게 다시 해봄



```python
```


2. 네트워크


```python
```



```python
```


```python
```
