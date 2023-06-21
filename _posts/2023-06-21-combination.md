---
layout: post
title: 백준 조합 - 11050번, 11051번, 2775번, 1010번, 13251번
subtitle: Baekjoon combination - 11050, 11051, 2775, 1010, 13251
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F06%2F20%2Flowest-common-ancestor.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 조합



## 백준 11050번

https://www.acmicpc.net/problem/11050

```python
import sys
input = sys.stdin.readline
N, K = map(int, input().split())
D = [[0 for j in range(N+1)] for i in range(N+1)]

for i in range(0, N+1):
    D[i][1] = i
    D[i][0] = 1
    D[i][i] = 1

for i in range(2, N+1):
    for j in range(1, i):
        D[i][j] = D[i-1][j] + D[i-1][j-1]

print(D[N][K])
```



<br>





## 백준 11051번

https://www.acmicpc.net/problem/11051

```python
import sys
input = sys.stdin.readline
N, K = map(int, input().split())
D = [[0 for j in range(N+1)] for i in range(N+1)]

for i in range(0, N+1):
    D[i][1] = i
    D[i][0] = 1
    D[i][i] = 1

for i in range(2, N+1):
    for j in range(1, i):
        D[i][j] = D[i-1][j] + D[i-1][j-1]
        D[i][j] = D[i][j] % 10007

print(D[N][K])
```



<br>







## 백준 2775번

https://www.acmicpc.net/problem/2775

```python
import sys
input = sys.stdin.readline
D = [[0 for j in range(15)] for i in range(15)]

for i in range(1, 15):
    D[i][1] = 1
    D[0][i] = i

for i in range(1, 15):
    for j in range(2, 15):
        D[i][j] = D[i][j-1] + D[i-1][j]

T = int(input())

for i in range(0, T):
    K = int(input())
    N = int(input())
    print(D[K][N])
```



<br>





## 백준 1010번

https://www.acmicpc.net/problem/1010

```python
import sys
input = sys.stdin.readline
D = [[0 for j in range(31)] for i in range(31)]

for i in range(0, 31):
    D[i][1] = i
    D[i][0] = 1
    D[i][i] = 1

for i in range(2, 31):
    for j in range(1, i):
        D[i][j] = D[i-1][j] + D[i-1][j-1]

t = int(input())

for i in range(0, t):
    N, M = map(int, input().split())
    print(D[M][N])
```



<br>





## 백준 13251번

https://www.acmicpc.net/problem/13251

```python
D = [0] * 51
probability = [0] * 51
M = int(input())
D = list(map(int, input().split()))
T = 0

for i in range(0, M):
    T += D[i]
    
K = int(input())
ans = 0

for i in range(0, M):
    if D[i] >= K:
        probability[i] = 1
        for k in range(0, K):
            probability[i] = probability[i] * (D[i] - k) / (T-k)
        ans += probability[i]

print(ans)
```



<br>





지금까지 백준 알고리즘 조합 부분이었습니다.

읽어주셔서 감사합니다. 😃

