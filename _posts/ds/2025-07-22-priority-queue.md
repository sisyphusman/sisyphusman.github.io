---
layout: post
title: 우선순위 큐(Priority Queue) [임시]
category: ds
---


# 우선순위 큐 
```python
import heapq

pq = []
heapq.heappush(pq, (2, "task2"))
heapq.heappush(pq, (1, "task1"))
heapq.heappush(pq, (3, "task3"))

while pq:
    priority, task = heapq.heappop(pq)
    print(priority, task)

from queue import PriorityQueue
pq = PriorityQueue()
pq.put((2, "task2"))
print(pq.get())
```
