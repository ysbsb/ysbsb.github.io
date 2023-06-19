---
layout: post
title: 백준 세그먼트 트리 - 2042번, 10868번, 11505번
subtitle: Baekjoon segment tree - 2042, 10868, 11505
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

# 1. 세그먼트 트리



## 백준 2042번

https://www.acmicpc.net/problem/2042

```python
import sys
input = sys.stdin.readline
N, M, K = map(int, input().split())
treeHeight = 0
lenght = N

while lenght != 0:
    lenght //= 2
    treeHeight += 1

treeSize = pow(2, treeHeight + 1)
leftNodeStartIndex = treeSize // 2 - 1
tree = [0] * (treeSize + 1)

for i in range(leftNodeStartIndex + 1, leftNodeStartIndex + N + 1):
    tree[i] = int(input())

def setTree(i):
    while i != 1:
        tree[i // 2] += tree[i]
        i -= 1

setTree(treeSize - 1)


def changeVal(index, value):
    diff = value - tree[index]
    while index > 0:
        tree[index] = tree[index] + diff
        index = index // 2

def getSum(s, e):
    partSum = 0
    while s <= e:
        if s % 2 == 1:
            partSum += tree[s]
            s += 1
        if e % 2 == 0:
            partSum += tree[e]
            e -= 1
        s = s // 2
        e = e // 2
    return partSum

for _ in range(M + K):
    question, s, e = map(int, input().split())
    if question == 1:
        changeVal(leftNodeStartIndex + s, e)
    elif question == 2:
        s = s + leftNodeStartIndex
        e = e + leftNodeStartIndex
        print(getSum(s, e))
```



<br>





## 백준 10868번

https://www.acmicpc.net/problem/10868

```python
import sys
input = sys.stdin.readline
N, M = map(int, input().split())
treeHeight = 0
lenght = N

while lenght != 0:
    lenght //= 2
    treeHeight += 1

treeSize = pow(2, treeHeight + 1)
leftNodeStartIndex = treeSize // 2 - 1
tree = [sys.maxsize] * (treeSize + 1)

for i in range(leftNodeStartIndex + 1, leftNodeStartIndex + N + 1):
    tree[i] = int(input())


def setTree(i):
    while i != 1:
        if tree[i // 2] > tree[i]:
            tree[i // 2] = tree[i]
        i -= 1

setTree(treeSize - 1)

def getMin(s, e):
    Min = sys.maxsize
    while s <= e:
        if s % 2 == 1:
            Min = min(Min, tree[s])
            s += 1
        if e % 2 == 0:
            Min = min(Min, tree[e])
            e -= 1
        s = s // 2
        e = e // 2
    return Min

for _ in range(M):
    s, e = map(int, input().split())
    s = s + leftNodeStartIndex
    e = e + leftNodeStartIndex
    print(getMin(s, e))
```



<br>





## 백준 11505번

https://www.acmicpc.net/problem/11505

```python
import sys
input = sys.stdin.readline
N, M, K = map(int, input().split())
treeHeight = 0
lenght = N
MOD = 1000000007;

while lenght != 0:
    lenght //= 2
    treeHeight += 1

treeSize = pow(2, treeHeight + 1)
leftNodeStartIndex = treeSize // 2 - 1
tree = [1] * (treeSize + 1)

for i in range(leftNodeStartIndex + 1, leftNodeStartIndex + N + 1):
    tree[i] = int(input())


def setTree(i):
    while i != 1:
        tree[i // 2] = tree[i // 2] * tree[i] % MOD
        i -= 1

setTree(treeSize - 1)

def changeVal(index, value):
    tree[index] = value
    while index > 1:
        index = index // 2
        tree[index] = tree[index * 2] % MOD * tree[index * 2 + 1] % MOD

def getMul(s, e):
    partMul = 1
    while s <= e:
        if s % 2 == 1:
            partMul = partMul * tree[s] % MOD
            s += 1
        if e % 2 == 0:
            partMul = partMul * tree[e] % MOD
            e -= 1
        s = s // 2
        e = e // 2
    return partMul

for _ in range(M + K):
    question, s, e = map(int, input().split())
    if question == 1:
        changeVal(leftNodeStartIndex + s, e)
    elif question == 2:
        s = s + leftNodeStartIndex
        e = e + leftNodeStartIndex
        print(getMul(s, e))
```



<br>





지금까지 백준 알고리즘 세그먼트 트리 부분이었습니다.

읽어주셔서 감사합니다. 😃

