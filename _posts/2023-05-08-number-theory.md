---
layout: post
title: 백준 정수론 1 - 1929번, 1456번, 1747번, 1016번, 11689번
subtitle: Baekjoon number theory 1 - 1929, 1456, 1747, 1016, 11689
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



## 백준 1929번

https://www.acmicpc.net/problem/1929



```python
import math
M, N = map(int, input().split())
A = [0]*(N+1)

for i in range(2, N+1):
    A[i] = i
    
for i in range(2, int(math.sqrt(N)) + 1):
    if A[i] == 0:
        continue
    for j in range(i+i, N+1, i):
        A[j] = 0

for i in range(M, N+1):
    if A[i] != 0:
        print(A[i])
```



<br>

## 백준 1456번



https://www.acmicpc.net/problem/1456



```python
import math
Min, Max = map(int, input().split())
A = [0] * (10000001)

for i in range(2, len(A)):
    A[i] = i
    
for i in range(2, int(math.sqrt(len(A))+1)):
    if A[i] == 0:
        continue
    for j in range(i+i, len(A), i):
        A[j] = 0
        
count = 0

for i in range(2, 10000001):
    if A[i] != 0:
        temp = A[i]
        while A[i] <= Max / temp:
            if A[i] >= Min / temp:
                count += 1
            temp = temp * A[i]
            
print(count)
```



<br>



## 백준 1747번



https://www.acmicpc.net/problem/1747



```python
import math
N = int(input())
A = [0] * (10000001)

for i in range(2, len(A)):
    A[i] = i
    
for i in range(2, int(math.sqrt(len(A))+1)):
    if A[i] == 0:
        continue
    for j in range(i+i, len(A), i):
        A[j] = 0
        
def isPalindrome(target):
    temp = list(str(target))
    s = 0
    e = len(temp) - 1
    while (s < e):
        if temp[s] != temp[e]:
            return False
        s += 1
        e -= 1
    return True

i = N
while True:
    if A[i] != 0:
        result = A[i]
        if (isPalindrome(result)):
            print(result)
            break
    i += 1

```



<br>

## 백준 1016번



https://www.acmicpc.net/problem/1016



```python
import math
Min, Max = map(int, input().split())
Check = [False] * (Max - Min + 1)

for i in range(2, int(math.sqrt(Max) +1)):
    pow = i * i
    start_index = int(Min/pow)
    if Min % pow != 0:
        start_index += 1
    for j in range(start_index, int(Max/pow)+1):
        Check[int((j*pow)-Min)] = True

count = 0

for i in range(0, Max - Min + 1):
    if not Check[i]:
        count += 1

print(count)
```



<br>

## 백준 11689번



https://www.acmicpc.net/problem/11689



```python
import math
N = int(input())
result = N

for p in range(2, int(math.sqrt(N))+1):
    if N%p == 0:
        result -= result / p
        while N%p == 0:
            N /= p
            
if N > 1:
    result -= result / N

print(int(result))
```



<br>



지금까지 백준 알고리즘 정수론 1 부분이었습니다.

읽어주셔서 감사합니다. 😃

