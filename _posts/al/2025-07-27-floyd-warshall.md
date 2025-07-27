---
layout: post
title: 플로이드 워셜(floyd-warshall)
category: al
---

그래프에서 모든 정점 쌍 사이의 최단 거리를 구해주는 알고리즘이다(all-to-all)

**방향** 그래프 혹은 **무방향** 그래프 모두 사용 가능하다

간선의 값이 음수여도 문제 없지만 음수인 **사이클**이 있으면 안된다

거리를 계산한 최소 비용인 테이블이 필요하다

X에서 Y로 가는 최소 비용과

X에서 어떤 노드를 거쳐서 가는 비용을 비교하여

거리의 **최소값**을 찾아 갱신해준다

그래프가 풍부한 나열 간선 수를 가지는 **조밀** 그래프에 적합

음수 가중치는 허용하지만 **음수** 사이클 존재 시 결과 신뢰 불가능하므로 탐지 **필요**


#### 코드 리뷰

```python
INF = float('inf')
V = 4  # 정점 수

# 거리 배열 초기화
dist = [[INF] * (V + 1) for _ in range(V + 1)]
for i in range(1, V + 1):
    dist[i][i] = 0  # 자기 자신까지는 0

# 간선 정보
edges = [
    (1, 2, 4),
    (1, 4, 10),
    (2, 3, 3),
    (3, 4, 1),
]

# 초기 간선 거리 설정
for a, b, cost in edges:
    dist[a][b] = cost

# 플로이드 와샬 알고리즘
for k in range(1, V + 1):
    for i in range(1, V + 1):
        for j in range(1, V + 1):
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])

# 결과 출력
print("최단 거리 행렬:")
for i in range(1, V + 1):
    for j in range(1, V + 1):
        if dist[i][j] == INF:
            print("INF", end="\t")
        else:
            print(dist[i][j], end="\t")
    print()

```

1. 초기 dist[i][j]를 직접 간선 비용이나 무한대로 초기화  
2. 자기 자신으로 가는 비용 dist[i][i] = 0 
3. 세 개의 반복문(k, i, j)을 통해  
   dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]) 갱신  
   - k는 중간 경유 노드, i와 j는 시작/도착 노드
4. 알고리즘 종료 후 dist[i][i] < 0인 경우 → 음수 사이클 존재 


#### 시간 복잡도

$$ O(V^3) $$(정점 수의 세제곱)

- 세 개의 중첩된 반복문(중간 노드, 시작 노드, 도착 노드)

**정점** 수가 500 이하일 때 실전 사용 가능

**간선** 수에 상관없이 정점 수에만 의존

#### 공간 복잡도

$$ O(V^2) $$(정점 쌍 거리 테이블 저장)