---
layout: post
title: 백준 정수론 2 - 1934번, 1850번, 1033번
subtitle: Baekjoon number theory 2 - 1934, 1850, 1033
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F05%2F08%2Fnumber-theory.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. Number theory



## 백준 1934번

https://www.acmicpc.net/problem/1934



```python
def gcd(a, b):
    if b == 0:
        return a
    else:
        return gcd(b, a%b)

t = int(input())

for i in range(t):
    a, b = map(int, input().split())
    result = a*b / gcd(a, b)
    print(int(result))
```



<br>

## 백준 1850번



https://www.acmicpc.net/problem/1850



```python
def gcd(a, b):
    if b == 0:
        return a
    else:
        return gcd(b, a%b)

a, b = map(int, input().split())
result = gcd(a, b)

while result > 0:
    print(1, end='')
    result -= 1
```



<br>



## 백준 1033번



https://www.acmicpc.net/problem/1033



```python
N = int(input())
A = [[] for _ in range(N)]
visited = [False] * (N)
D = [0]*(N)
lcm = 1

def gcd(a, b):
    if b==0:
        return a
    else:
        return gcd(b, a%b)

def DFS(v):
    visited[v] = True
    for i in A[v]:
        next = i[0]
        if not visited[next]:
            D[next] = D[v]*i[2]//i[1]
            DFS(next)
        
for i in range(N-1):
    a, b, p, q = map(int, input().split())
    A[a].append((b,p,q))
    A[b].append((a,q,p))
    lcm *= (p*q//gcd(p, q))
    
D[0] = lcm
DFS(0)
mgcd = D[0]

for i in range(1, N):
    mgcd = gcd(mgcd, D[i])

for i in range(N):
    print(int(D[i]//mgcd), end=' ')
```



<br>



지금까지 백준 알고리즘 정수론 2 부분이었습니다.

읽어주셔서 감사합니다. 😃

