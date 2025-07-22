---
layout: post
title: 재귀(Recursion) [임시]
category: al
---

# 재귀 함수
```python
def fact(n):
    if n <= 1:
        return 1
    return n * fact(n - 1)
```