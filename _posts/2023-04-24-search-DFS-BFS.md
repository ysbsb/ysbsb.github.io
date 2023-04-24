---
layout: post
title: 백준 DFS, BFS - 2023번, 13023번, 1260번, 2178번, 1167번
subtitle: Baekjoon DFS, BFS - 2023, 13023, 1260, 2178, 1167
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F04%2F24%2Fsearch-DFS-BFS.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. DFS

## 백준 2023번

https://www.acmicpc.net/problem/2023



```python
import sys
sys.setrecursionlimit(10000)
input = sys.stdin.readline
N = int(input())

def isPrime(num):
    for i in range(2, int(num/2 + 1)):
        if num % i == 0:
            return False
    return True

def DFS(number):
    if len(str(number)) == N:
        print(number)
    else:
        for i in range(1, 10):
            if i % 2 == 0:
                continue
            if isPrime(number*10 + i):
                DFS(number * 10 + i)
                
DFS(2)
DFS(3)
DFS(5)
DFS(7)
```



<br>

## 백준 1940번



https://www.acmicpc.net/problem/1940



```python
import sys
input = sys.stdin.readline
N = int(input())
M = int(input())
A = list(map(int, input().split()))
A.sort()
count = 0
i = 0
j = N-1

while i<j:
    if A[i] + A[j] < M:
        i += 1
    elif A[i] + A[j] > M:
        j -= 1
    else:
        count += 1
        i += 1
        j -= 1
        
print(count)
```



<br>



## 백준 13023번



https://www.acmicpc.net/problem/13023



```python
import sys
sys.setrecursionlimit(10000)
input = sys.stdin.readline
N, M = map(int, input().split())
arrive = False
A = [[] for _ in range(N+1)]
visited [False] * (N + 1)

def DFS(now, depth):
    global arrive
    if depth == 5:
        arrive = True
        return
    visited[now] = True
    for i in A[now]:
        if not visited[i]:
            DFS(i, depth+1)
    visited[now] = False
    
for _ in range(M):
    s, e = map(int, input().split())
    A[s].append(e)
    A[e].append(s)

for i in range(N):
    DFS(i, 1)
    if arrive:
        break

if arrive:
    print(1)
else:
    print(0)
```



<br>





# 2. BFS

## 백준 1260번



https://www.acmicpc.net/problem/1260



```python
from collections import deque
N, M, Start = map(int, input().split())
A = [[] for _ in range(N+1)]

for _ in range(M):
    s, e = map(int, input().split())
    A[s].append(e)
    A[e].append(s)
    
for i in range(N+1):
    A[i].sort()
    
def DFS(v):
    print(v, end=' ')
    visited[v] = True
    for i in A[v]:
        if not visited[i]:
            DFS(i)
            
visited = [False] * (N+1)
DFS(Start)

def BFS(v):
    queue = deque()
    queue.append(v)
    visited[v] = True
    while queue:
        now_Node = queue.popleft()
        print(now_Node, end=' ')
        for i in A[now_Node]:
            if not visited[i]:
                visited[i] = True
                queue.append(i)

print()
visited = [False] * (N+1)
BFS(Start)
```

<br>



## 백준 2178번



https://www.acmicpc.net/problem/2178



```python
from collections import deque

dx = [0, 1, 0, -1]
dy = [1, 0, -1, 0]
N, M = map(int, input().split())
A = [[0]*M for _ in range(N)]
visited = [[False] * M for _ in range(N)]

for i in range(N):
    numbers = list(input())
    for j in range(M):
        A[i][j] = int(numbers[j])
        
def BFS(i, j):
    queue = deque()
    queue.append((i, j))
    visited[i][j] = True
    while queue:
        now = queue.popleft()
        for k in range(4):
            x = now[0] + dx[k]
            y = now[1] + dy[k]
            if x>= 0 and y >= 0 and x<N and y<M:
                if A[x][y]!= 0 and not visited[x][y]:
                    visited[x][y] = True
                    A[x][y] = A[now[0]][now[1]] + 1
                    queue.append((x, y))
                    
BFS(0, 0)
print(A[N-1][M-1])
```



<br>





## 백준 1167번



https://www.acmicpc.net/problem/1167



```python
from collections import deque

N = int(input())
A = [[] for _ in range(N+1)]

for _ in range(N):
    Data = list(map(int, input().split()))
    index = 0
    S = Data[index]
    index += 1
    while True:
        E = Data[index]
        if E == -1:
            break
        V = Data[index + 1]
        A[S].append((E, V))
        index += 2

distance = [0] * (N+1)
visited = [False] * (N+1)

def BFS(v):
    queue = deque()
    queue.append(v)
    visited[v] = True
    while queue:
        now_Node = queue.popleft()
        for i in A[now_Node]:
            if not visited[i[0]]:
                visited[i[0]] = True
                queue.append(i[0])
                distance[i[0]] = distance[now_Node] + i[1]
                
BFS(1)
Max = 1

for i in range(2, N+1):
    if distance[Max] < distance[i]:
        Max = i

distance = [0]*(N+1)
visited = [False]*(N+1)
BFS(Max)
distance.sort()
print(distance[N])
```



<br>



지금까지 백준 알고리즘 DFS, BFS 부분이었습니다.

읽어주셔서 감사합니다. 😃

