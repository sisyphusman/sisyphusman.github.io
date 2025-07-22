---
layout: post
title: 이분 탐색(Binary Search) [임시]
category: al
---

```python
def binary_search(arr, target, left, right):
    while left <= right:
        mid = (left + right) // 2

        if arr[mid] == target:
            return mid  # 또는 True 등으로 반환
        elif arr[mid] < target:
            left = mid + 1  # 오른쪽 범위 탐색
        else:
            right = mid - 1  # 왼쪽 범위 탐색

    return False  # 찾지 못했을 때
```