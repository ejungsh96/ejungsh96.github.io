---
layout: single
title:  "LeetCode 57"
categories: Coding
tags:
  - python
  - data structure
  - algorithm
toc: true
toc_sticky: true
---

💡 Data Structure and Algorithm Practice

# [완주하지 못한 선수](https://programmers.co.kr/learn/courses/30/lessons/42576)

### Possible Algorithms

* Hash

```python
from collections import Counter
 
def solution(participant, completion):
    
    anwer = Counter(participant) - Counter(completion)
    
    return list(anwer.keys())[0]
```

# [K번째수](https://programmers.co.kr/learn/courses/30/lessons/42748)

### Possible Algorithms

* Sort

```python
def solution(array, commands):
    res = []
    for com in commands:
        res.append(sorted(array[com[0] - 1 : com[1]])[com[2] - 1])
    
    return res
```

# [체육복](https://programmers.co.kr/learn/courses/30/lessons/42862)

### Possible Algorithms

* Greedy

```python
def solution(n, lost, reserve):
    # # Initial Try - error
    # saved = len(lost)
    # for std in lost:
    #     if std in reserve:
    #         reserve.remove(std)
    #     if std - 1 in reserve:
    #         saved -= 1
    #         reserve.remove(std - 1)
    #     elif std + 1 in reserve:
    #         saved -= 1
    #         reserve.remove(std + 1)
    
    # return n - saved
 
    set_reserve = set(reserve) - set(lost)
    set_lost = set(lost) - set(reserve)
 
    for i in set_reserve:
	    if i - 1 in set_lost:
		    set_lost.remove(i - 1)
	    elif i + 1 in set_lost:
		    set_lost.remove(i + 1)
 
    return n - len(set_lost)
```

# [가장 먼 노드](https://programmers.co.kr/learn/courses/30/lessons/49189?language=python3#)

### Possible Algorithms

* Dijkstra Algorithm
* BFS

```python
import heapq
from collections import deque
 
def solution(n, edge):
    # Dijkstra Algorithm - find the shortest path from a node to every other nodes.
    graph = [[] for _ in range(n + 1)]
    
    for x, y in edge:
        graph[x].append((y, 1))
        graph[y].append((x, 1))
    
    distance = [float('inf')] * (n + 1)
    distance[1] = 0
    
    q = []
    heapq.heappush(q, (0, 1))   # (cost, node)
    
    def go(start):  # start -> 1
        while q:
            dist, now = heapq.heappop(q)
            
            if dist > distance[now]:
                continue
            
            # Update distance
            for i in graph[now]:
                cost = dist + i[1]
                if cost < distance[i[0]]:
                    distance[i[0]] = cost
                    heapq.heappush(q, (cost, i[0]))
    
    go(1)
    max_dist = max(distance[1:])
    return distance.count(max_dist)
 
 
    # BFS
    graph = [[] for _ in range(n + 1)]
    for x, y in edge:
        graph[x].append(y)
        graph[y].append(x)
        
    visited = [-1] * (n + 1)
    
    def bfs(node, visited, graph):
        count = 0
        q = deque([(node, count)])
        
        while q:
            cur = q.popleft()
            node, count = cur
            if visited[node] == -1:
                visited[node] = count
                count += 1
                for adj in graph[node]:
                    q.append((adj, count))
                
    res = 0
    bfs(1, visited, graph)
    for value in visited:
        if value == max(visited):
            res += 1
    
    return res
```

# [N으로 표현](https://programmers.co.kr/learn/courses/30/lessons/42895?language=python3)

### Possible Algorithms

* Dynamic Programming

```python
def solution(N, number):
    answer = -1
    if number == N:
        return 1
    
    _li = [set() for _ in range(8)]
    for i in range(len(_li)):
        _li[i].add(int(str(N) * (i + 1)))
    
    for i in range(1, 8):
        for j in range(i):
            for op1 in _li[j]:
                for op2 in _li[i - j - 1]:
                    _li[i].add(op1 + op2)
                    _li[i].add(op1 - op2)
                    _li[i].add(op1 * op2)
                    if op2 != 0:
                        _li[i].add(op1 // op2)
        if number in _li[i]:
            answer = i + 1
            break
    
    return answer
```

# [모의고사](https://programmers.co.kr/learn/courses/30/lessons/42840?language=python3)

### Possible Algorithms

* Brute Force

```python
def solution(answers):
    answer = []
    l = len(answers)
    p_1 = [1, 2, 3, 4, 5]
    p_2 = [2, 1, 2, 3, 2, 4, 2, 5]
    p_3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]
    answers_1 = p_1 * (l // 5) + p_1[:(l % 5)]
    answers_2 = p_2 * (l // 8) + p_2[:(l % 8)]
    answers_3 = p_3 * (l // 10) + p_3[:(l % 10)]
    
    grades = []
    for i in [answers_1, answers_2, answers_3]:
        count = 0
        for a1, a2 in zip(i, answers):
            if a1 == a2:
                count += 1
        grades.append(count)
    
    _max = max(grades)
    return [grades.index(_max) + 1] if grades.count(_max) == 1 else [x + 1 for x in range(len(grades)) if grades[x] == _max]
```

# [타겟넘버](https://programmers.co.kr/learn/courses/30/lessons/43165?language=python3)

### Possible Algorithms

* DFS

```python
def solution(numbers, target):
    answer = 0
    queue = [[numbers[0] , 0], [-1 * numbers[0] , 0]]
    n = len(numbers)
    while queue:
        temp, idx = queue.pop()
        idx += 1
        if idx < n:
            queue.append([temp + numbers[idx], idx])
            queue.append([temp - numbers[idx], idx])
        else:
            if temp == target:
                answer += 1
    return answer
 
    # Recursion
    n = len(numbers)
    answer = 0
    def dfs(idx, result):
        if idx == n:
            if result == target:
                nonlocal answer
                answer += 1
            return
        else:
            dfs(idx+1, result+numbers[idx])
            dfs(idx+1, result-numbers[idx])
    dfs(0,0)
    return answer
```
