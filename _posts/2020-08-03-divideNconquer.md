---
layout: post
title:  "[Python] 백준 알고리즘 분할정복(divide & conquer): 문제풀이 11728, 1780, 11729, 1992, 2447, 1517, 2261 "
date:   2020-08-03
categories: algorithm
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
n = int(input())
def hanoi(n, start, temp, end):
    path = []
    if n==1:
        path.append([start, end])
    else:
        tp = hanoi(n-1, start, end, temp) # step 1) n-1개를 temp번칸으로 이동
        for t in tp:
            path.append(t)

        path.append([start, end]) # step 2) 마지막을 end칸으로 이동

        tp = hanoi(n-1, temp, start, end) # step 3) temp번칸에 있던 n-1개를 end번칸으로 이동
        for t in tp:
            path.append(t)

    return path

result = hanoi(n, 1, 2, 3)
print(len(result))
for r in result:
    for i in r:
        print(i, end=" ")
    print()
```


<br>

[1992: 쿼드트리](https://www.acmicpc.net/problem/1992)이걸 풀고 나니까 분할정복에 대한 감을 완조니 잡은 기분이당 ~!

```python
def check_squard(lst):
    standard = lst[0][0]
    result = []
    same = True
    if len(lst)>1:
        for i in range(len(lst)):
            if same:
                for j in range(len(lst)):
                    if lst[i][j]!=standard:
                        same = False
                        break
                # cut
        if not same:
            for t in cut(lst): result.append(check_squard(t))               

    if len(lst)==1 or same:
        return standard

    return tuple(result)

def cut(lst):
    result = []
    n = int(len(lst)/2)
    result.append([lst[i][:n] for i in range(n)])
    result.append([lst[i][n:] for i in range(n)])
    result.append([lst[i][:n] for i in range(n,len(lst))])
    result.append([lst[i][n:] for i in range(n,len(lst))])
    return result


n = int(input())
data = []
for i in range(n):
    data.append([int(i) for i in input()])

print((str(check_squard(data))).replace(",", '').replace(' ',''))
```

<br>

[2447](https://www.acmicpc.net/problem/2447)은 별찍기문제당

```python
def stars(result, row1, row2, col1, col2):
    size = row2-row1  # 또는 col2-col1
    n = int(size/3)
    for i in range(n,2*n):
        for j in range(n,2*n):
            result[row1+i][col1+j] = ' '
    if n>1:
        stars(result, row1, row1+n, col1, col1+n)
        stars(result, row1+n, row1+2*n, col1, col1+n)
        stars(result, row1+2*n, row2, col1, col1+n)

        stars(result, row1, row1+n, col1+n, col1+2*n)
        stars(result, row1+2*n, row2, col1+n, col1+2*n)

        stars(result, row1, row1+n, col1+2*n, col2)
        stars(result, row1+n, row1+2*n, col1+2*n, col2)
        stars(result, row1+2*n, row2, col1+2*n, col2)

N = int(input())
result = [["*"]*N for _ in range(N)]
stars(result, 0, N, 0, N)
for i in range(len(result)):
    print(''.join(result[i]))
```



<br>

[1517: bubble sort](https://www.acmicpc.net/problem/1517)도 풀어봤당

위에서 푼 `11728`은 아마 merge sort 인듯

```python
def bubble(arr, r1, r2):
    if arr[r1]>arr[r2]:
        #print('swap with '+ str(arr[r1]) + 'with' + str(arr[r2]) )
        temp = arr[r1]
        arr[r1] = arr[r2]
        arr[r2] = temp
        return 1
    return 0

n= int(input())
nums = [int(i) for i in input().split()]

count = 0
n_len = len(nums)
while (n_len !=0):
    for i in range(n_len-1):
        count += bubble(nums, i, i+1)
    n_len-=1
print(count)
```

처음에 이렇게 풀었는데,,, 시간 초과가 떠버렸당


<br>

```python
n= int(input())
nums = [int(i) for i in input().split()]

count = 0
for i in range(len(nums), 1, -1):
    for j in range(i-1):
        if nums[j]>nums[j+1]:
            count +=1
            nums[j], nums[j+1] = nums[j+1], nums[j]
print(count)
```

이후에는 python의 swap 기능을 이용해봤지만 이것도 시간초과 ,,, 왜지



<br>



그래서 `divide & conquer` 라는 개념 자체를 놓친다는 생각이 들어서,

O(N^2)를 어떻게 하면 그 이하로 줄일 수 있을까 고민했당


휴 결국은 bubble sort 로 구현을 한다면 시간초과가 나는 문제,,,

bubble sort에서 swap 되는 조건을 이용하여 merge sort를 이용해야한다,,,

(아니 그럴거면 왜 bubble sort 라고 한거야 ! )


```python
import sys
sys.setrecursionlimit(10 ** 9)
def merge(start, mid, end):
    arr = []
    i1, i2 = start, mid
    count = 0
    while i1 < mid and i2 < end:
        if nums[i1] > nums[i2]:
            arr.append(nums[i2])
            i2 += 1
            count += 1
        else:
            arr.append(nums[i1])
            i1 += 1
            swap += count

    while i1 < mid:
        arr.append(nums[i1])
        i1 += 1
        swap += count
    while i2 < mid:
        arr.append(nums[i2])
        i2 += 1

    for t in range(len(nums)):
        nums[start + t] = arr[t]

def bubble(start, end):
    if start != end:
        mid = int((start+end)/2)
        bubble(start, mid)
        bubble(mid, end)
        merge(start, mid, end)        

n= int(input())
nums = [i for i in input().split()]
swap = 0
bubble(0, n-1)
print(count)        
```


<br>

[2261](https://www.acmicpc.net/problem/2261)은 가장 가까운 두 점 !

이것도 자꾸 시간초과, 메모리초과 떠서 봤더니 x좌표를 기준으로 나누어야 한다도라,,,

즉, x 좌표의 중간값을 기준으로 잡고 아래와 같이 case 분류를 한당 ( [여기](https://www.acmicpc.net/blog/view/25) 참고 ! )



 - case 1) 두 점이 모두 기준보다 왼쪽에 존재

 - case 2) 두 점이 각각 왼쪽, 오른쪽에 존재

 - case 3) 두 점이 모두 기준보다 오른쪽에 존재



```python
```



<br>
