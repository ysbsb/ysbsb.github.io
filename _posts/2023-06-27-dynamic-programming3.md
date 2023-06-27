---
layout: post
title: 백준 동적 계획법 3 - 1915번, 1328번, 2342번
subtitle: Baekjoon dynamic programming 3 - 1915, 1328, 2342
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

# 1. 동적 계획법 3



## 백준 1915번

https://www.acmicpc.net/problem/1915

```python
import sys
input = sys.stdin.readline
D = [[0 for j in range(1001)] for i in range(1001)]
N, M = map(int, input().split())
max = 0

for i in range(0, N):
    numbers = list(input())
    for j in range(0, M):
        D[i][j] = int(numbers[j])
        if D[i][j] == 1 and j >0 and i>0:
            D[i][j] = min(D[i-1][j-1], D[i-1][j], D[i][j-1]) + D[i][j]
        if max < D[i][j]:
            max = D[i][j]

print(max*max)
```



<br>





## 백준 1328번

https://www.acmicpc.net/problem/1328

```python
import sys
input = sys.stdin.readline
mod = 1000000007
N, L, R = map(int, input().split())
D = [[[0 for k in range(101)] for j in range(101)] for i in range(101)]
D[1][1][1] = 1

for i in range(2, N+1):
    for j in range(1, L+1):
        for k in range(1, R+1):
            D[i][j][k] = (D[i-1][j][k]*(i-2) + (D[i-1][j][k-1] + D[i-1][j-1][k])) % mod

print(D[N][L][R])
```



<br>







## 백준 2342번

https://www.acmicpc.net/problem/2342

```python
import sys
input = sys.stdin.readline

dp = [[[sys.maxsize for k in range(5)] for j in range(5)] for i in range(100001)]

mp = [[0, 2, 2, 2, 2],
      [2, 1, 3, 4, 3],
      [2, 3, 1, 3, 4],
      [2, 4, 3, 1, 3],
      [ 2, 3, 4, 3, 1]]

s = 1
dp[0][0][0] = 0

inputList =  list(map(int, input().split()))
index = 0

while inputList[index] != 0:
    n = inputList[index]
    for i in range(5):
        if n == i:
            continue
        for j in range(5):
            dp[s][i][n] = min(dp[s - 1][i][j] + mp[j][n], dp[s][i][n])

    for j in range(5):
        if n == j:
            continue
        for i in range(5):
            dp[s][n][j] = min(dp[s - 1][i][j] + mp[i][n], dp[s][n][j])
    s += 1
    index += 1
s -= 1
result = sys.maxsize
for i in range(5):
    for j in range(5):
        result = min(result, dp[s][i][j])
print(result)




```



<br>









지금까지 백준 알고리즘 동적 계획법 3 부분이었습니다.

읽어주셔서 감사합니다. 😃

