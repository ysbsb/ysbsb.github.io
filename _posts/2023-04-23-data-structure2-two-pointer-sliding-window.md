---
layout: post
title: 백준 투포인터, 슬라이딩 윈도우 - 2018번, 1940번, 1253번, 12891번, 11003번
subtitle: Baekjoon two pointer, sliding window - 2018, 1940, 1253, 12891, 11003
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F04%2F23%2Fdata-structure1-array-list-sum.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 투포인터

## 백준 2018번

https://www.acmicpc.net/problem/2018



```python
n = int(input())
count = 1
start_index = 1
end_index = 1
sum = 1

while end_index != n:
    if sum == n:
        count += 1
        end_index += 1
        sum += end_index
    elif sum > n:
        sum -= start_index
        start_index += 1
    else:
        end_index += 1
        sum += end_index
        
print(count)
```



<br>

## 백준 1940번



https://www.acmicpc.net/problem/1940



```python
import sys
input = sys.stdin.readline
N = int(input())
M = int(input())
A = list(map(int, input().split()))
A.sort()
count = 0
i = 0
j = N-1

while i<j:
    if A[i] + A[j] < M:
        i += 1
    elif A[i] + A[j] > M:
        j -= 1
    else:
        count += 1
        i += 1
        j -= 1
        
print(count)
```



<br>



## 백준 1253번



https://www.acmicpc.net/problem/1253



```python
import sys
input = sys.stdin.readline
N = int(input())
Result = 0
A = list(map(int, input().split()))
A.sort()

for k in range(N):
    find = A[k]
    i = 0
    j = N-1
    while i < j:
        if A[i] + A[j] == find:
            if i != k and j != k:
                Result += 1
                break
            elif i == k:
                i += 1
            elif j == k:
                j -= 1
        elif A[i] + A[j] < find:
            i += 1
        else:
            j -= 1
            
print(Result)
```



<br>





# 2. 슬라이딩 윈도우

## 백준 12891번



https://www.acmicpc.net/problem/12891



```python
import sys
input = sys.stdin.readline


checkList = [0]*4
myList [0]*4
checkSecret = 0

def myadd(c):
    global checkList, myList, checkSecret
    if c == 'A':
        myList[0] += 1
        if myList[0] == checkList[0]:
            checkList += 1
    elif c == 'C':
        myList[1] += 1
        if myList[1] == checkList[1]:
            checkList += 1
    elif c == 'G':
        myList[2] += 1
        if myList[2] == checkList[2]:
            checkList += 1
    elif c == 'T':
        myList[3] += 1
        if myList[3] == checkList[3]:
            checkList += 1

def myremove(c):
    global checkList, myList, checkSecret
    if c == 'A':
        if myList[0]== checkList[0]:
            checkSecret -= 1
        myList[0] -= 1
    elif c == 'C':
        if myList[1]== checkList[1]:
            checkSecret -= 1
        myList[1] -= 1
    elif c == 'G':
        if myList[2]== checkList[2]:
            checkSecret -= 1
        myList[2] -= 1
    elif c == 'T':
        if myList[3]== checkList[3]:
            checkSecret -= 1
        myList[3] -= 1

S, P = map(int, input().split())
Result = 0
A = list(input())
checkList = list(map(int, input().split()))

for i in range(4):
    if checkList[i] == 0:
        checkSecret += 1
        
for i in range(P):
    myadd(A[i])
    
if checkSecret == 4:
    Result += 1
    
for i in range(P, S):
    j = i - P
    myadd(A[i])
    myremove(A[j])
    if checkSecret == 4:
        Result += 1
        
print(Result)
            
```

<br>



## 백준 11003번



https://www.acmicpc.net/problem/11003



```python
from collections import deque
N, L = map(int, input().split())
mydeque = deque()
now = list(map(int, input().split()))

for i in range(N):
    while mydeque and mydeque[-1][0] > now[i]:
        mydeque.pop()
    mydeque.append((now[i], i))
    if mydeque[0][1] <= i - L:
        mydeque.popleft()
    print(mydeque[0][0], end=' ')
```



<br>



지금까지 백준 알고리즘 투포인터, 슬라이딩 윈도우 부분이었습니다.

읽어주셔서 감사합니다. 😃

