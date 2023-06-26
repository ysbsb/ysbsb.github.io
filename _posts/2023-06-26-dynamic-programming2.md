---
layout: post
title: 백준 동적 계획법 2 - 10844번, 13398번, 9252번
subtitle: Baekjoon dynamic programming 2 - 10844, 13398, 9252s
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true
---











> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F06%2F26%2Fdynamic-programming2.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 동적 계획법 2



## 백준 10844번

https://www.acmicpc.net/problem/10844

```python
import sys
input = sys.stdin.readline
mod = 1000000000
N = int(input())
D = [[0 for j in range(11)] for i in range(N+1)]

for i in range(1, 10):
    D[1][i] = 1
    
for i in range(2, N+1):
    D[i][0] = D[i-1][1]
    D[i][9] = D[i-1][8]
    for j in range(1, 9):
        D[i][j] = (D[i-1][j-1] + D[i-1][j+1]) % mod

sum = 0

for i in range(10):
    sum = (sum + D[N][i]) % mod
    
print(sum)
```



<br>





## 백준 13398번

https://www.acmicpc.net/problem/13398

```python
N = int(input())
A = list(map(int, input().split()))

L = [0] * N
L[0] = A[0]
result = L[0]

for i in range(1, N):
    L[i] = max(A[i], L[i-1] + A[i])
    result = max(result, L[i])

R = [0] * N
R[N-1] = A[N-1]

for i in range(N-2, -1, -1):
    R[i] = max(A[i], R[i+1]+A[i])

for i in range(1, N-1):
    temp = L[i-1] + R[i+1]
    result = max(result, temp)

print(result)
```



<br>







## 백준 9252번

https://www.acmicpc.net/problem/9252

```python
import sys
sys.setrecursionlimit(10000)
input = sys.stdin.readline
A = list(input())
A.pop()
B = list(input())
B.pop()

DP = [[0 for j in range(len(B)+1)] for i in range(len(A)+1)]
Path = []

for i in range(1, len(A) + 1):
    for j in range(1, len(B) + 1):
        if A[i-1] == B[j-1]:
            DP[i][j] = DP[i-1][j-1] + 1
        else:
            DP[i][j] = max(DP[i-1][j], DP[i][j-1])

print(DP[len(A)][len(B)])

def getText(r, c):
    if r==0 or c==0:
        return
    if A[r-1] == B[c-1]:
        Path.append(A[r-1])
        getText(r-1, c-1)
    else:
        if DP[r-1][c] > DP[r][c-1]:
            getText(r-1, c)
        else:
            getText(r, c-1)
            
getText(len(A), len(B))

for i in range(len(Path)-1, -1, -1):
    print(Path.pop(i), end='')
    
print()

```



<br>









지금까지 백준 알고리즘 동적 계획법 2 부분이었습니다.

읽어주셔서 감사합니다. 😃

