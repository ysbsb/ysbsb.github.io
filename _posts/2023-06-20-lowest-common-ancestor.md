---
layout: post
title: 백준 최소 공통 조상 - 11437번, 11438번
subtitle: Baekjoon lowest common ancestor - 11437, 11438
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F06%2F19%2Fsegment-tree.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 최소 공통 조상



## 백준 11437번

https://www.acmicpc.net/problem/11437

```python
import sys
input = sys.stdin.readline
N = int(input())
tree = [[] for _ in range(N+1)]

for _ in range(0, N-1):
    s, e = map(int, input().split())
    tree[s].append(e)
    tree[e].append(s)

depth = [0]*(N+1)
parent = [0]*(N+1)
visited = [False]*(N+1)

def BFS(node):
    queue = [node]
    visited[node] = True
    while queue:
        now_node = queue.pop(0)
        for next in tree[now_node]:
            if not visited[next]:
                visited[next] = True
                queue.append(next)
                parent[next] = now_node
                depth[next] = depth[now_node]+1
       
BFS(1)

def excuteLCA(a, b):
    if depth[a] < depth[b]:
        temp = a
        a = b
        b = temp

    while depth[a] != depth[b]:
        a = parent[a]

    while a != b:
        a = parent[a]
        b = parent[b]

    return a

M = int(input())
mydict = dict()
for _ in range(M):
    a, b = map(int, input().split())
    if not mydict.get((a, b), 0):
        mydict[(a, b)] = mydict[(b, a)] = excuteLCA(a, b)
    print(mydict.get((a, b)))
```



<br>





## 백준 11438번

https://www.acmicpc.net/problem/11438

```python
import sys
from collections import deque
input = sys.stdin.readline
print = sys.stdout.write
N = int(input())
tree = [[0] for _ in range(N+1)]

for _ in range(0, N-1):
    s, e = map(int, input().split())
    tree[s].append(e)
    tree[e].append(s)
    
depth = [0]*(N+1)
visited = [False]*(N+1)
temp = 1
kmax = 0

while temp <= N:
    temp <<=1
    kmax += 1
    
parent = [[0 for j in range(N+1)] for i in range(kmax+1)]

def BFS(node):
    queue = deque()
    queue.append(node)
    visited[node] = True
    level = 1
    now_size = 1
    count = 0
    while queue:
        now_node = queue.popleft()
        for next in tree[now_node]:
            if not visited[next]:
                visited[next] = True
                queue.append(next)
                parent[0][next] = now_node
                depth[next] = level
        count += 1
        if count == now_size:
            count = 0
            now_size = len(queue)
            level += 1

BFS(1)

for k in range(1, kmax+1):
    for n in range(1, N+1):
        parent[k][n] = parent[k-1][parent[k-1][n]]

def executeLCA(a, b):
    if depth[a] > depth[b]:
        temp = a
        a = b
        b = temp
        
    for k in range(kmax, -1, -1):
        if pow(2, k) <= depth[b] - depth[a]:
            if depth[a] <= depth[parent[k][b]]:
                 b = parent[k][b]
                    
    for k in range(kmax, -1, -1):
        if a==b: break
        if parent[k][a] != parent[k][b]:
            a = parent[k][a]
            b = parent[k][b]
    
    LCA = a
    if a != b:
        LCA = parent[0][LCA]
    return LCA

M = int(input())

for _ in range(M):
    a, b = map(int, input().split())
    print(str(executeLCA(a, b)))
    print("\n")
```



<br>







지금까지 백준 알고리즘 최소 공통 조상 부분이었습니다.

읽어주셔서 감사합니다. 😃

