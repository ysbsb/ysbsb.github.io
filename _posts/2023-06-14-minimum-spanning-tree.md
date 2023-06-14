---
layout: post
title: 백준 최소 신장 트리 - 1197번, 17472번, 1414번
subtitle: Baekjoon minimum spanning tree - 1197, 17472, 1414
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F06%2F14%2Fminimum-spanning-tree.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 최소 신장 트리



## 백준 1197번

https://www.acmicpc.net/problem/1197

```python
import sys
from queue import PriorityQueue

input = sys.stdin.readline
N, M = map(int, input().split())
pq = PriorityQueue()
parent = [0] * (N+1)

for i in range(N+1):
    parent[i] = i
    
for i in range(M):
    s, e, w = map(int, input().split())
    pq.put((w, s, e))

def find(a):
    if a == parent[a]:
        return a
    else:
        parent[a] = find(parent[a])
        return parent[a]

def union(a, b):
    a = find(a)
    b = find(b)
    if a!=b:
        parent[b] = a
        
useEdge = 0
result = 0

while useEdge < N-1:
    w, s, e = pq.get()
    if find(s) != find(e):
        union(s, e)
        result += w
        useEdge += 1

print(result)
```



<br>

## 백준 17472번



https://www.acmicpc.net/problem/17472

```python
import copy
import sys
from collections import deque
from queue import PriorityQueue
input = sys.stdin.readline

dr = [0,1,0,-1]
dc = [1,0,-1,0]

N, M = map(int, input().split())
myMap = [[0 for j in range(M)] for i in range(N)]
visited = [[False for j in range(M)] for i in range(N)]

for i in range(N):
    myMap[i] = list(map(int, input().split()))
    
sNum = 1
sumlist = list([])
mlist = []

def addNode(i, j, queue):
    myMap[i][j] = sNum
    visited[i][j] = True
    temp = [i, j]
    mlist.append(temp)
    queue.append(temp)

def BFS(i, j):
    queue = deque()
    mlist.clear()
    start = [i, j]
    queue.append(start)
    mlist.append(start)
    visited[i][j] = True
    myMap[i][j] = sNum
    
    while queue:
        r, c = queue.popleft()
        for d in range(4):
            tempR = dr[d]
            tempC = dc[d]
            while r+tempR >=0 and r+tempR < N and c+tempC >= 0 and c+tempC < M:
                if not visited[r+tempR][c+tempC] and myMap[r+tempR][c+tempC] != 0:
                    addNode(r+tempR, c+tempC, queue)
                else:
                    break
                if tempR < 0:
                    tempR -= 1
                elif tempR > 0:
                    tempR += 1
                elif tempC < 0:
                    tempC -=1
                elif tempC > 0:
                    tempC += 1
    return mlist

for i in range(N):
    for j in range(M):
        if myMap[i][j] != 0 and not visited[i][j]:
            tempList = copy.deepcopy(BFS(i, j))
            sNum += 1
            sumlist.append(tempList)

pq = PriorityQueue()

for now in sumlist:
    for temp in now:
        r = temp[0]
        c = temp[1]
        now_S = myMap[r][c]
        for d in range(4):
            tempR = dr[d]
            tempC = dc[d]
            blength = 0
            while r+tempR>=0 and r+tempR<N and c+tempC>=0 and c+tempC<M:
                if myMap[r+tempR][c+tempC] == now_S:
                    break
                elif myMap[r+tempR][c+tempC] != 0:
                    if blength > 1:
                        pq.put((blength, now_S, myMap[r+tempR][c+tempC]))
                    break
                else:
                    blength += 1
                if tempR < 0:
                    tempR -= 1
                elif tempR > 0:
                    tempR += 1
                elif tempC < 0:
                    tempC -= 1
                elif tempC > 0:
                    tempC += 1

def find(a):
    if a == parent[a]:
        return a
    else:
        parent[a] = find(parent[a])
        return parent[a]
    
def union(a, b):
    a = find(a)
    b = find(b)
    if a!=b:
        parent[b] = a

parent = [0] * sNum

for i in range(len(parent)):
    parent[i] = i
    
useEdge = 0
result = 0

while pq.qsize() > 0:
    v, s, e = pq.get()
    if find(s) != find(e):
        union(s, e)
        result += v
        useEdge += 1

if useEdge == sNum - 2:
    print(result)
else:
    print(-1)


```



<br>



## 백준 1414번



https://www.acmicpc.net/problem/1414



```python
import sys
from queue import PriorityQueue

input = sys.stdin.readline
N = int(input())
pq = PriorityQueue()
sum = 0
for i in range(N):
    tempc = list(input())
    for j in range(N):
        temp = 0
        if 'a' <= tempc[j] <= 'z':
            temp = ord(tempc[j]) - ord('a') + 1
        elif 'A' <= tempc[j] <= 'Z':
            temp = ord(tempc[j]) - ord('A') + 27
        sum += temp
        if i != j and temp != 0:
            pq.put((temp, i, j))

parent = [0] * N
for i in range(N):
    parent[i] = i


def find(a):
    if a == parent[a]:
        return a
    else:
        parent[a] = find(parent[a])
        return parent[a]


def union(a, b):
    a = find(a)
    b = find(b)
    if a != b:
        parent[b] = a


useEdge = 0
result = 0
while pq.qsize() > 0:
    v, s, e = pq.get()
    if find(s) != find(e):
        union(s, e)
        result += v
        useEdge += 1

if useEdge == N - 1:
    print(sum - result)
else:
    print(-1)
```



<br>





지금까지 백준 알고리즘 최소 신장 트리 부분이었습니다.

읽어주셔서 감사합니다. 😃

