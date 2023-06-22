---
layout: post
title: 백준 조합 2 - 1722번, 1256번, 1947번
subtitle: Baekjoon combination 2 - 1722, 1256, 1947
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F06%2F22%2Fcombination2.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 조합



## 백준 1722번

https://www.acmicpc.net/problem/1722

```python
import sys
input = sys.stdin.readline

F = [0] * 21
S = [0] * 21
visited = [False] * 21
N = int(input())
F[0] = 1

for i in range(1, N+1):
    F[i] = F[i-1] * i

inputList = list(map(int, input().split()))

if inputList[0] == 1:
    K = inputList[1]
    for i in range(1, N+1):
        cnt = 1
        for j in range(1, N+1):
            if visited[j]:
                continue
            if K <= cnt*F[N-i]:
                K -= (cnt-1) * F[N-i]
                S[i] = j
                visited[j] = True
                break
            cnt += 1
    for i in range(1, N+1):
        print(S[i], end=' ')
else:
    K = 1
    for i in range(1, N+1):
        cnt = 0
        for j in range(1, inputList[i]):
            if not visited[j]:
                cnt += 1
        K += cnt * F[N-i]
        visited[inputList[i]] = True
    print(K)

```



<br>





## 백준 1256번

https://www.acmicpc.net/problem/1256

```python
import sys
input = sys.stdin.readline
N, M, K = map(int, input().split())
D = [[0 for j in range(202)] for i in range(202)]

for i in range(0, 201):
    for j in range(0, i+1):
        if j==0 or j==i:
            D[i][j] = 1
        else:
            D[i][j] = D[i-1][j-1]+D[i-1][j]
            if D[i][j]>1000000000:
                D[i][j] = 1000000001

if D[N+M][M] < K:
    print(-1)
else:
    while not (N==0 and M==0):
        if D[N-1+M][M] >= K:
            print("a", end='')
            N -= 1
        else:
            print("z", end='')
            K -= D[N-1+M][M]
            M -= 1

```



<br>







## 백준 1947번

https://www.acmicpc.net/problem/1947

```python
N = int(input())
mod = 1000000000
D = [0]*1000001
D[1] = 0
D[2] = 1

for i in range(3, N+1):
    D[i] = (i-1)* (D[i-1] + D[i-2]) % mod
    
print(D[N])
```



<br>









지금까지 백준 알고리즘 조합 2 부분이었습니다.

읽어주셔서 감사합니다. 😃

