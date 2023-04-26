---
layout: post
title: 백준 이진 탐색 - 1920번, 2343번, 1300번
subtitle: Baekjoon binary seach - 1920, 2343, 1300
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

# 1. Binary Search

## 백준 1920번

https://www.acmicpc.net/problem/1920



```python
N = int(input())
A = list(map(int, input().split()))
A.sort()
M = int(input())
target_list = list(map(int, input().split()))

for i in range(M):
    find = False
    target = target_list[i]
    start = 0
    end = len(A) - 1
    while start <= end:
        midi = int((start+end)/2)
        midv = A[midi]
        if midv > target:
            end = midi - 1
        elif midv < target:
            start = midi + 1
        else:
            find = True
            break
            
    if find:
        print(1)
    else:
        print(0)
```



<br>

## 백준 2343번



https://www.acmicpc.net/problem/2343



```python
N, M = map(int, input().split())
A = list(map(int, input().split()))
start = 0
end = 0

for i in A:
    if start < i:
        start = i
    end += i
    
while start <= end:
    middle = int((start+end)/2)
    sum = 0
    count = 0
    for i in range(N):
        if sum + A[i] > middle:
            count += 1
            sum = 0
        sum += A[i]
    if sum != 0:
        count += 1
    if count > M:
        start = middle+1
    else:
        end = middle -1
        
print(start)
```



<br>



## 백준 1300번



https://www.acmicpc.net/problem/1300



```python
N = int(input())
K = int(input())
start = 1
end = K
ans = 0

while start <= end:
    middle = int((start + end) / 2)
    cnt = 0

    for i in range(1, N + 1):
        cnt += min(int(middle / i), N)
    if cnt < K:
        start = middle + 1
    else:
        ans = middle
        end = middle - 1
print(ans)
```





<br>



지금까지 백준 알고리즘 이진 탐색 부분이었습니다.

읽어주셔서 감사합니다. 😃

