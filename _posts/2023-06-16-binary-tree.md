---
layout: post
title: 백준 이진 트리 - 1991번
subtitle: Baekjoon binary tree - 1991
category: [algorithm]
tags: [algorithm]
comments: true
use_math: true

---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Falgorithm%2F2023%2F06%2F16%2Fbinary-tree.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅에서는 백준 알고리즘 문제 풀이에 대해 포스팅하도록 하겠습니다. 

<br>

코드는 [이곳](https://github.com/doitcodingtest/python)을 참고했음을 밝힙니다.

<br>

# 1. 이진 트리



## 백준 1991번

https://www.acmicpc.net/problem/1991

```python
import sys
input = sys.stdin.readline
N = int(input())
tree = {}

for _ in range(N):
    root, left, right = input().split()
    tree[root] = [left, right]
    
def preOrder(now):
    if now == '.':
        return
    print(now, end='')
    preOrder(tree[now][0])
    preOrder(tree[now][1])

def inOrder(now):
    if now == '.':
        return
    inOrder(tree[now][0])
    print(now, end='')
    inOrder(tree[now][1])

def postOrder(now):
    if now == '.':
        return
    postOrder(tree[now][0])
    postOrder(tree[now][1])
    print(now, end='')

preOrder('A')
print()
inOrder('A')
print()
postOrder('A')
```



<br>





지금까지 백준 알고리즘 이진 트리 부분이었습니다.

읽어주셔서 감사합니다. 😃

