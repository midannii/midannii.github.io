---
layout: post
title:  "[Programmers] 알고리즘 오랜만에 복습 [5] DP, sort - n으로 표현, 가장 큰 수  "
date:   2020-10-28
categories: algorithm
---
<br>


1. N으로 표현 [DP]

DP,,, 사칙연산으로 dp 하는 건 너무 생소해

[참고 1](https://www.hamadevelop.me/algorithm-n-expression/)

[참고 2](https://gurumee92.tistory.com/164)

[참고 3](https://velog.io/@imacoolgirlyo/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-N%EC%9C%BC%EB%A1%9C-%ED%91%9C%ED%98%84-%ED%8C%8C%EC%9D%B4%EC%8D%AC)

```python
def solution(N, number):
    '''
    숫자 N과 number가 주어질 때,
    N과 사칙연산만 사용해서 number를 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return
    수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시
    최솟값이 8보다 크면 -1을 return

    # https://velog.io/@imacoolgirlyo/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-N%EC%9C%BC%EB%A1%9C-%ED%91%9C%ED%98%84-%ED%8C%8C%EC%9D%B4%EC%8D%AC
    '''
    S = [0, {N}]
    for i in range(2, 9):
        case_set = {int(str(N)*i)}
        for i_half in range(1, i//2+1):  # S[i_half] S[1]
            for x in S[i_half]:
                for y in S[i-i_half]:
                    case_set.add(x+y)
                    case_set.add(x-y)
                    case_set.add(y-x) # y-x 케이스 추가
                    case_set.add(x*y)
                    if x != 0:
                        case_set.add(y//x)
                    if y != 0:
                        case_set.add(x//y)
        if number in case_set:
            return i
        S.append(case_set)
    return -1
```

2. 가장 큰 수 [정렬]


처음에는 이렇게 풀었는데

```python
def solution(numbers):
    strnum = []
    for n in numbers: strnum.append(str(n))
    strnum.sort(reverse = True)
    answer = ''
    for n in strnum: answer +=n
    return answer
```

풀고보니 30 이 3 보다 크다고 하는 거야 ㅠㅠ 나름 트릭을 생각했다고 생각했는데

역시 이렇게 간단할리가 없엉

```python
#https://hocheon.tistory.com/48
def solution(numbers):
    # number -> str
    numbers = list(map(str, numbers))
    # x*3이라는 람다함수를 구현함으로서, 더욱 쉽게 비교 가능
    numbers.sort(key = lambda x: x*3, reverse=True)
    return str(int(''.join(numbers)))
```
