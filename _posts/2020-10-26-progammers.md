---
layout: post
title:  "[Programmers] 알고리즘 오랜만에 복습 [1] Binary Search - 입국심사 "
date:   2020-10-26
desc: " "
keywords: "python, Programmers, algorithm"
categories: [Python]
tags: [python, algorithm, BinarySearch]
icon: icon-html
---
<br>
너무 오랜만에 풀어봐서 진짜로 다 까먹은 듯한 binary search ㅠㅠ 문제를 들여다보다가 이게 왜 BS 지...? 싶었다

아마도 내가 제대로 이해를 못해서 그런걸까,,, 엉엉


<br>

1. 입국심사

처음에 작성한 코드는 아래와 같은데, test case는 통과했지만 답은 틀렸당

```Python

def solution(n, times):
    '''
    input : 입국심사를 기다리는 사람 수 n,
            각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times
    '''
    answer = 0
    # 한번에 심사를 받을 수 있는 경우
    if n<=len(times): answer = times[n-1]
    # 한번에 심사를 받을 수 없는 경우
    else:
        if n%2 == 0:
            temp1 = solution(n-2, times)+times[1]
            temp2 = solution(n-1, times)+times[0]
            if temp1 > temp2:
                answer = temp2
            else: answer = temp1
        else: answer = times[0]* (int(n/len(times))+1)
    return answer
```

아마도 `심사관은 1명 이상 100,000명 이하` 라는 조건에 틀려서 그런듯 !


```Python

def solution(n, times):
    '''
    input : 입국심사를 기다리는 사람 수 n,
            각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times
    '''
    answer = 0
    # 한번에 심사를 받을 수 있는 경우
    if n<=len(times): answer = times[n-1]
    # 한번에 심사를 받을 수 없는 경우
    else:
        if n%len(times) == 0:
            min_test = 10000
            for i in range(len(times)):
                if min_test > solution(n-i, times)+times[i-1]:
                    min_test = solution(n-i, times)+times[i-1]
            answer = min_test
        else: answer = times[len(times)%n-1]*(int(n/len(times))+1)
    return answer
```

요렇게 작성해보니 또 recursion limit에 걸렸고,,
