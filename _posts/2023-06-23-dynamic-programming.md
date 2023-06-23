---
layout: post
title: 백준 동적 계획법 1 - 1463번, 14501번, 2193번, 11726번
subtitle: Baekjoon dynamic programming - 1463, 14501, 2193, 11726
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true
---











> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F06%2F23%2Fdynamic-programming.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 동적 계획법



## 백준 1463번

https://www.acmicpc.net/problem/1463

```python
import sys
input = sys.stdin.readline
N = int(input())
D = [0]*(N+1)
D[1] = 0

for i in range(2, N+1):
    D[i] = D[i-1] + 1
    if i % 2 == 0:
        D[i] = min(D[i], D[i//2]+1)
    if i % 3 == 0:
        D[i] = min(D[i], D[i//3]+1)

print(D[N])
```



<br>





## 백준 14501번

https://www.acmicpc.net/problem/14501

```python
import sys
input = sys.stdin.readline
N = int(input())
D = [0]*(N+2)
T = [0]*(N+1)
P = [0]*(N+1)

for i in range(1, N+1):
    T[i], P[i] = map(int, input().split())
    
for i in range(N, 0, -1):
    if i + T[i] > N+1:
        D[i] = D[i+1]
    else:
        D[i] = max(D[i+1], P[i]+D[i+T[i]])
        
print(D[1])
```



<br>







## 백준 2193번

https://www.acmicpc.net/problem/2193

```python
import sys
input = sys.stdin.readline
N = int(input())
D = [[0 for j in range(2)] for i in range(N+1)]
D[1][1] = 1
D[1][0] = 0

for i in range(2, N+1):
    D[i][0] = D[i-1][1] + D[i-1][0]
    D[i][1] = D[i-1][0]
    
print(D[N][0] + D[N][1])
```



<br>



## 백준 11726번

https://www.acmicpc.net/problem/11726

```python
mod = 10007
N =int(input())
D = [0] * (1001)
D[1] = 1
D[2] = 2

for i in range(3, N+1):
    D[i] = (D[i-1]+D[i-2]) % mod

print(D[N])
```



<br>









지금까지 백준 알고리즘 동적 계획법 부분이었습니다.

읽어주셔서 감사합니다. 😃

