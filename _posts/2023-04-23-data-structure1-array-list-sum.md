---
layout: post
title: 백준 배열, 리스트, 구간합 - 11720번, 1546번, 11659번, 11660번
subtitle: Baekjoon array, list, sum - 11720, 1546, 11659, 11660
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Fnas%2F2022%2F08%2F22%2FPNAS.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 배열과 리스트

## 백준 11720번

https://www.acmicpc.net/problem/11720



```python
n = input()
numbers = list(input())
sum = 0

for i in numbers:
    sum += int(i)
   
print(sum)
```



<br>

## 백준 1546번



https://www.acmicpc.net/problem/1546



```python
n = input()
mylist = list(map(int, input().split()))
mymax = max(mylist)
sum = sum(mylist)
print(sum*100/mymax/int(n))
```



<br>









# 2. 구간 합

## 백준 11659번



https://www.acmicpc.net/problem/11659



```python
import sys
input = sys.stdin.readline
suNo, quizNo = map(int, input().split())
numbers = list(map(int, input().split()))
prefix_sum = [0]
temp = 0

for i in numbers:
    temp = temp + i
    prefix_sum.append(temp)
    
for i in range(quizNo):
    s, e = map(int, input().split())
    print(prefix_sum[e] - prefix_sum[s-1])
```

<br>



## 백준 11660번



https://www.acmicpc.net/problem/11660



```python
import sys
input = sys.stdin.readline
n, m = map(int, input().split())
A = [[0*(n+1)]]
D = [[0] * (n+1) for _ in range(n+1)]

for i in range(n):
    A_row = [0] + [int(x) for x in input().split()]
    A.append(A_row)
    
for i in range(1, n+1):
    for j in range(1, n+1):
        D[i][j] = D[i][j-1] + D[i-1][j] - D[i-1][j-1] + A[i][j]
        
for _ in range(m):
    x1, y1, x2, y2 = map(int, input().split())
    result = D[x2][y2] - D[x1-1][y2] - D[x2][y1-1] + D[x1-1][y1-1]
    print(result)
```



<br>



지금까지 백준 알고리즘 배열, 리스트, 구간합 부분이었습니다.

읽어주셔서 감사합니다. 😃

