---
layout: post
title: 이분 탐색[임시]
category: ad
---

```python
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2

        if array[mid] == target:
            return True
        elif array[mid] > target:
            end = mid - 1
        else:
            start = mid + 1

    return False
```