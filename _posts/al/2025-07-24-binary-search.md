---
layout: post
title: 이진(이분) 탐색(Binary Search)
category: al
---
#### 이진탐색 vs 선형검색 단순 비교

![이분탐색 vs 선형검색](/assets/images/al/binary-search-01.gif)

&nbsp;

&nbsp;

```python
def binary_search(arr, target, left, right):
    while left <= right:
        mid = (left + right) // 2

        if arr[mid] == target:
            return True  # 또는 True 등으로 반환
        elif arr[mid] < target:
            left = mid + 1  # 오른쪽 범위 탐색
        else:
            right = mid - 1  # 왼쪽 범위 탐색

    return False  # 찾지 못했을 때
```

&nbsp;

#### 코드 리뷰

```python
(binary_search(arr, target, 0, n - 1)) 
```

  전형적인 binary search는 인자로는 

  (검색할 배열, 찾을 값, 시작 범위, 끝 범위) 4개의 값을 받는다

  &nbsp;

  ![코드리뷰-1](/assets/images/al/binary-search-03.png) 

  요소가 5개인 배열을 넘겨주었을때 배열 인덱스는 0~4이다

  left는 왼쪽 경계, right는 오른쪽 경계라고 가정한다

  경계가 같아지거나 넘어갈때는 찾는 요소가 없다 (return False)

  &nbsp;

  ![코드리뷰-2](/assets/images/al/binary-search-04.png) 
  
  mid 값은 left와 right을 더한 값에 절반이다

  mid가 타겟값이 아니면 다음 회전때 
  
  좌우 경계를 변경하여 타겟 값을 향하여 범위를 이동한다

  mid가 타겟값이면 루프를 종료한다 (return True)

  &nbsp;

#### 시간 복잡도

  정렬된 배열에만 이진 탐색을 사용할 수 있다

  정렬되지 않는 배열에서는 잘못된 방향으로 갈 수 있기 때문이다

  - **최선의 경우**: O(1) — 한 번에 타겟을 찾을 때
  - **평균적인 경우**: O(log N)
  - **최악의 경우**: O(log N) — 탐색 대상이 없거나 마지막까지 비교해야 할 때  
    

  배열에 16개의 요소가 있을때 최악의 경우 4번 비교를 해야한다

  &nbsp;

#### 이진탐색이 worst case인 경우

![이분탐색 나쁜 예](/assets/images/al/binary-search-02.gif)