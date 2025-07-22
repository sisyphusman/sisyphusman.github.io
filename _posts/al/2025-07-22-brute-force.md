---
layout: post
title: 완전 탐색(Brute Force) [임시]
category: al
---

# 완전 탐색
```python
def two_sum(nums, target):
    n = len(nums)
    for i in range(n):
        for j in range(i+1, n):
            if nums[i] + nums[j] == target:
                return (i, j)
    return None
```