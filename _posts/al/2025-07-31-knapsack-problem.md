---
layout: post
title: 배낭 문제(Knapsack Problem)
category: al
---

배낭 문제
- 제한된 무게의 배낭(knapsack)이 있고 각각 **무게와 가치**가 주어진 아이템 중  
**가치의 합이 최대**가 되도록 선택하는 **조합 최적화 문제**

문제 정의
- 배낭의 최대 무게: W
- 각 아이템의 무게: weights = [w1, w2, ..., wn]
- 각 아이템의 가치: values = [v1, v2, ..., vn]
- 목표: 총 무게 <= W 조건하에 가치의 합을 최대화

예시
- 도둑의 배낭은 최대 15kg까지 담을 수 있다

  | 상자 | 무게(kg) | 가치($) |
  |------|----------|----------|
  | A    | 12       | 4        |
  | B    | 1        | 2        |
  | C    | 4        | 10       |
  | D    | 1        | 1        |
  | E    | 2        | 2        |

&nbsp;

#### 분할 가능한 배낭 문제
- 아이템을 부분적으로 쪼개서 담을 수 있는 경우

최적 해
- C(4kg) + B(1kg) + E(2kg) + D(1kg) + A(7kg 일부만)
- 총 무게 = 15kg
- 총 가치 = 최대(약 17.33)

#### 코드 리뷰

그리디 알고리즘(Greedy)으로 해결

```python
# (무게, 가치)
items = [(12, 4), (1, 2), (4, 10), (1, 1), (2, 2)]
capacity = 15

# 가치/무게 비율로 정렬 (내림차순)
items.sort(key=lambda x: x[1]/x[0], reverse=True)

total_value = 0
for weight, value in items:
    if capacity >= weight:
        total_value += value
        capacity -= weight
    else:
        # 남은 무게만큼 비율로 담기
        total_value += value * (capacity / weight)
        break

print(f"Fractional Knapsack 총 가치: {total_value}")
```

#### 시간 복잡도
- n - 아이템의 수

- Fractional Knapsack: O(n log n)

#### 0-1 배낭 문제
- 아이템을 쪼갤 수 없고 전체를 통째로 넣거나 아예 안 넣거나 해야 하는 경우

최적 해
- C(4kg) + B(1kg) + E(2kg) + D(1kg)
- A(12kg)는 너무 무거워서 배낭에 안 들어감
- 총 무게 = 8kg  
- 총 가치 = 최대(15)

동적 계획법(DP)로 해결

#### 코드 리뷰
```python
# (무게, 가치)
items = [(12, 4), (1, 2), (4, 10), (1, 1), (2, 2)]
capacity = 15
n = len(items)

# DP 테이블 생성
dp = [[0] * (capacity + 1) for _ in range(n + 1)]

# DP 채우기
for i in range(1, n + 1):
    w, v = items[i - 1]
    for c in range(capacity + 1):
        if c < w:
            dp[i][c] = dp[i - 1][c]
        else:
            dp[i][c] = max(dp[i - 1][c], dp[i - 1][c - w] + v)

print(f"0-1 Knapsack 총 가치: {dp[n][capacity]}")
```

#### 시간 복잡도
- n - 아이템의 수

- W - 배낭의 최대 용량

- 0-1 Knapsack: O(n × W)