---
layout: post
title: 백준 동적 계획법 4 - 11049번, 2098번, 14003번
subtitle: Baekjoon dynamic programming 4 - 11049, 2098, 14003
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true
---











> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F06%2F27%2Fdynamic-programming3.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 동적 계획법 4



## 백준 11049번

https://www.acmicpc.net/problem/11049

```python
import sys
input = sys.stdin.readline
N = int(input())
M = []
D = [[-1 for j in range(N+1)] for i in range(N+1)]

M.append((0,0))

for i in range(N):
    x, y = map(int, input().split())
    M.append((x, y))
    
def execute(s, e):
    result = sys.maxsize
    if D[s][e] != -1:
        return D[s][e]
    if s==e:
        return 0
    if s+1==e:
        return M[s][0] * M[s][1] * M[e][1]
    for i in range(s, e):
        result = min(result, M[s][0]*M[i][1]*M[e][1] + execute(s, i) + execute(i+1, e))
    D[s][e] = result
    return D[s][e]

print(execute(1, N))
```



<br>





## 백준 2098번

https://www.acmicpc.net/problem/2098

```python
import sys
input = sys.stdin.readline
N = int(input())
W = []

for i in range(N):
    W.append([])
    W[i] = list(map(int, input().split()))

D = [[0 for j in range(1<<16)] for i in range(16)]

def tsp(c, v):
    if v == (1<<N) - 1:
        if W[c][0] == 0:
            return float('inf')
        else:
            return W[c][0]
    if D[c][v] != 0:
        return D[c][v]
    min_val = float('inf')
    for i in range(0, N):
        if (v& (1<<i)) ==0 and W[c][i] != 0:
            min_val = min(min_val, tsp(i, (v|(1<<i))) + W[c][i])
    D[c][v] = min_val
    return D[c][v]

print(tsp(0, 1))
```



<br>







## 백준 14003번

https://www.acmicpc.net/problem/14003

```python
import sys
input = sys.stdin.readline
N = int(input())
A = list(map(int, input().split()))
A.insert(0, 0)

index = 0
maxLength = 1
B = [0] * 1000001
D = [0] * 1000001
ans = [0] * 1000001
B[maxLength] = A[1]
D[1] = 1

def binarysearch(l, r, now):
    while l<r:
        mid = (l+r) // 2
        if B[mid] < now:
            l = mid + 1
        else:
            r = mid
    return l

for i in range(2, N+1):
    if B[maxLength] < A[i]:
        maxLength += 1
        B[maxLength] = A[i]
        D[i] = maxLength
    else:
        index = binarysearch(1, maxLength, A[i])
        B[index] = A[i]
        D[i] = index

print(maxLength)
index = maxLength
x = B[maxLength] + 1

for i in range(N, 0, -1):
    if D[i] == index:
        ans[index] = A[i]
        index -= 1

for i in range(1, maxLength+1):
    print(ans[i], end=' ')
```



<br>









지금까지 백준 알고리즘 동적 계획법 4 부분이었습니다.

읽어주셔서 감사합니다. 😃

