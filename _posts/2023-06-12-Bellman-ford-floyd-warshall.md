---
layout: post
title: 백준 벨만 포드, 플로이드 워셜 - 11657번, 1219번, 11404번, 11403번, 1389번
subtitle: Baekjoon Bellman Ford, Floyd Warshall - 11657, 1219, 11404, 11403, 1389
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F06%2F12%2FBellman-ford-floyd-warshall.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 벨만 포드



## 백준 11657번

https://www.acmicpc.net/problem/11657

```python
import sys
input = sys.stdin.readline
N, M = map(int, input().split())
edges = []
distance = [sys.maxsize]*(N+1)

for i in range(M):
    start, end, time = map(int, input().split())
    edges.append((start, end, time))
    
distance[1] = 0

for _ in range(N-1):
    for start, end, time in edges:
        if distance[start] != sys.maxsize and distance[end] > distance[start] + time:
            distance[end] = distance[start] + time
            
mCycle = False

for start, end, time in edges:
    if distance[start] != sys.maxsize and distance[end] > distance[start] + time:
        mCycle = True

if not mCycle:
    for i in range(2, N+1):
        if distance[i] != sys.maxsize:
            print(distance[i])
        else:
            print(-1)
else:
    print(-1)
```



<br>

## 백준 1219번



https://www.acmicpc.net/problem/1219

```python
import sys
input = sys.stdin.readline
N, sCity, eCity, M = map(int, input().split())
edges = []
distance = [-sys.maxsize] * N

for _ in range(M):
    start, end, price = map(int, input().split())
    edges.append((start, end, price))

cityMoney = list(map(int, input().split()))

distance[sCity] = cityMoney[sCity]

for i in range(N+101):
    for start, end, price in edges:
        if distance[start] == -sys.maxsize:
            continue
        elif distance[start] == sys.maxsize:
            distance[end] = sys.maxsize
        elif distance[end] < distance[start] + cityMoney[end] - price:
            distance[end] = distance[start] + cityMoney[end] - price
            if i >= N-1:
                distance[end] = sys.maxsize

if distance[eCity] == -sys.maxsize:
    print("gg")
elif distance[eCity] == sys.maxsize:
    print("Gee")
else:
    print(distance[eCity])
```



<br>



# 2. 플로이드 워셜



## 백준 11404번



https://www.acmicpc.net/problem/11404



```python
import sys
input = sys.stdin.readline
N = int(input())
M = int(input())
distance = [[sys.maxsize for j in range(N+1)] for i in range(N+1)]

for i in range(1, N+1):
    distance[i][i] = 0

for i in range(M):
    s, e, v = map(int, input().split())
    if distance[s][e] > v:
        distance[s][e] = v


for k in range(1, N+1):
    for i in range(1, N+1):
        for j in range(1, N+1):
            if distance[i][j] > distance[i][k] + distance[k][j]:
                distance[i][j] = distance[i][k] + distance[k][j]

for i in range(1, N+1):
    for j in range(1, N+1):
        if distance[i][j] == sys.maxsize:
            print(0, end=' ')
        else:
            print(distance[i][j], end=' ')
    print()

```



<br>



## 백준 11403번



https://www.acmicpc.net/problem/11403



```python
N = int(input())
distance = [[0 for j in range(N)] for i in range(N)]

for i in range(N):
    distance[i] = list(map(int, input().split()))
    
for k in range(N):
    for i in range(N):
        for j in range(N):
            if distance[i][k] == 1 and distance[k][j] ==1:
                distance[i][j] = 1
                
for i in range(N):
    for j in range(N):
        print(distance[i][j], end=' ')
    print()
```



<br>



## 백준 1389번



https://www.acmicpc.net/problem/1389



```python
import sys
N, M = map(int, input().split())
distance = [[sys.maxsize for j in range(N+1)] for i in range(N+1)]

for i in range(1, N+1):
    distance[i][i] = 0

for i in range(M):
    s, e = map(int, input().split())
    distance[s][e] = 1
    distance[e][s] = 1

for k in range(1, N+1):
    for i in range(1, N+1):
        for j in range(1, N+1):
            if distance[i][j] > distance[i][k] + distance[k][j]:
                distance[i][j] = distance[i][k] + distance[k][j]

Min = sys.maxsize
Answer = -1

for i in range(1, N+1):
    temp = 0
    for j in range(1, N+1):
        temp += distance[i][j]
    if Min > temp:
        Min = temp
        Answer = i
print(Answer)
```



<br>





지금까지 백준 알고리즘 벨만 포드, 플로이드 워셜 부분이었습니다.

읽어주셔서 감사합니다. 😃

