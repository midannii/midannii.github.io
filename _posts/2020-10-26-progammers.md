---
layout: post
title:  "[Programmers] 알고리즘 오랜만에 복습 [1] Binary Search - 입국심사, 징검다리 "
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
    times.sort()
    # 한번에 심사를 받을 수 있는 경우
    if n<=len(times): answer = times[n-1]
    # 한번에 심사를 받을 수 없는 경우
    else:
        if n%len(times) == 0:
            min_test = 10000
            for i in range(1,len(times)+1):
                #print(i)
                if min_test > solution(n-i, times)+times[i-1]:
                    min_test = solution(n-i, times)+times[i-1]
                    print(min_test)
            answer = min_test
            #print(answer)
        else:
            #print(times[n%len(times)-1])
            #print((int(n/len(times))+1))
            answer = times[n%len(times)-1]*(int(n/len(times))+1)
    return answer

```

이런저런 코드를 작성해봤는데도 여전히 너무 틀렸고,

한시간이 넘게 고민해도 해답을 찾을 수 없어서, 내가 binary search의 기준 및 어떤 값을 탐색해야 할지를 모르는 것 같아, 다른 분들의 풀이을 참고하였다.

아 앞으로 다른 분들의 풀이를 참고할 때에는 그냥 `오 그렇군!` 하고 넘기지 않고, 최소 3명의 코드를 비교해보고 스스로 짜봐야 겠다.. 안그러면 빠르게 까먹는듯 ㅠㅠ

[참고 1](https://wwlee94.github.io/category/algorithm/binary-search/immigration/)

[참고 2](https://post.naver.com/viewer/postView.nhn?volumeNo=27248090&memberNo=33264526)

[참고 3](https://inspirit941.tistory.com/entry/Python-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EC%9E%85%EA%B5%AD-%EC%8B%AC%EC%82%AC-Level-3)


위 참고 1, 2, 3에서는 공통적으로 아래와 같이 설정했다

- BS 의 기준: 주어진 시간동안 각 심사원이 심사를 했을 때, 심사를 마친 사람이 N명 이상인지 (이상이면 시간을 줄이고, 이하면 시간을 늘린다 )

- BS 할 값: 한명의 심사원에게 얼마의 시간을 줄 것인가


```Python
def solution(n, times):
    '''
    input : 입국심사를 기다리는 사람 수 n,
            각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times
    '''
    answer = 0
    left, right = 1, max(times)*n # 오래 걸리는 심사관에게 전부 받는 경우가 worst case
    while left<=right:
        # 한 심사관에게 주어진 경우
        mid = ((left+right)//2)
        people = 0 # 각 심사관이 제한 시간동안 심사할 수 있는 사람의 총 수

        for t in times: # 각 심사위원이
            people += mid//t # 주어진 시간동안 심사할 수 있는 사람의 수
            if people>=n: break # 전부 심사한 경우 종료

        if people >= n: # n명을 심사할 수 있는 경우, 시간을 줄여볾
            right = mid-1
            answer = mid
        else: # 시간안에 심사가 불가능한 경우, 시간을 늘림
            left = mid+1


    return answer
```



2. 징검다리


이번에는, 제일 먼저 binary search의 기준 및 수행할 값을 먼저 고민해보기로 했다!

(여전히 완전탐색으로만 접근하려는게 너무 힘들다 ㅠㅠ )

우선 처음으로 했던 접근은 최악의 시나리오였다! 바위 개수 == 제거해야할 개수라서, **간격의 최소값이 distance인 경우가 최악의 시나리오** 이므로,

우리가 binary search해야 하는 값은 `각 돌 사이의 거리` 이다.

이를 위해서는 **돌을 몇개를 어떻게 뺴야하는지**가 관건인 셈인데,

이를 binary search의 기준으로 표현해보면, `뺀 돌의 개수를 n과 비교하는 것` 이다.
