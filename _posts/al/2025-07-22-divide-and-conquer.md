---
layout: post
title: 분할 정복(Divide and Conquer) [임시]
category: al
---

# 분할 정복

```python

# 배열의 합 분할 정복

def sum_array(arr, left, right):
    if left == right:
        return arr[left]

    mid = (left + right) // 2
    left_sum = sum_array(arr, left, mid)        # 왼쪽 절반: left ~ mid 이다
    right_sum = sum_array(arr, mid + 1, right)  # 오른쪽 절반: mid + 1 ~ right이다

    return left_sum + right_sum
```
