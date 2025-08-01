---
layout: post
title : 정글 알고리즘 3강
category: al
---

> 아래 내용은 경기대 배상원 교수님의 강의에서 가져왔음을 밝힙니다

&nbsp;

# Divide-and-Conquer(분할 정복)

Mergesort
1. 주어진 배열을(대략 절반으로)둘로 나눈다
2. 두 subarray를 각각 정렬한다(by recursion)
3. 정렬된 두 subarray를 합쳐서(merge) 하나의 정렬된 배열로 만든다

단, 주어진 instance가 충분히 작으면 recursion 없이 바로 해결
- Base Case

D&C Examples: Max

- MAX
  - input: array A[0, n-1]
  - output: maximum element in A

최대값 찾는 함수는 O(N)보다 좋아질 수 없다 (N개를 다 봐야한다)

recursion tree의 노드 개수가 콜의 개수다

&nbsp;

#### Binary Search
탐색 문제 in sorted array
- input: sorted array A[0..n-1] and a key element k
- output: find the position of k in A, if exists, or report k is not in A

Binary Search(이진 탐색) algorithm
- A의 중간값(median)을 k와 비교하여 같으면 리턴
- k가 작으면 왼쪽 절반에 대해 탐색 계속
- k가 크면 오른쪽 절반에 대해 탐색 계속

Binary Search는 사실 D&C
- 중간값(median)을 k와 비교 한 후에 배열의 절반에 대해서만 탐색을 계속
- 즉, 남은 n/2 크기의 부분 배열에 대한 subproblem을 풀면 문제 해결
- k가 중간값보다 왼쪽에 있다/오른쪽에 있다가 아니라, 왼쪽 반에 존재하지 않는다는 사실이 중요하다.
  그래서 해당 절반을 버릴 수 있다
- 시간 복잡도 O(log n)

Multiplication
- input: two n-digit integers x and y
- output: product of x and y

$$ O(n^2) $$-time solvable
- Long multiplication, Lattice multiplication

Divide-and-Conquer 들이대기
- 어떻게 분할하지?
- 분할 후 subproblem을 풀고 나서는 어떻게 마무리하지?

  1472  
X 3986  
-------------    n = 4 절반을 잘라서 DC하기  
(1400 + 72)(3900 + 86)  
= (14 * 39) * 10000 + (14 * 86) * 100 + (72 * 39) * 1000 + 72 * 86  

- 정리해보면 x와 y를 반으로 잘라서

- ac, bc, ad, bd를 recursion으로 계산함

SplitMultiply의 Time Complexity
- recursion part: n/2 사이즈의 instance들 4번 recursive call
- non-recursion part:O(n) operations(덧셈과 shift 연산)
- T(n) = $$ O(n^2) $$

더 빠르게는 안 되는걸까?
- O(n^2) 시간이 최선인가
- 이것이 특성 문제 본질에 관한 질문

&nbsp;

#### 시간복잡도 분석
- Karatsuba 알고리즘
- Recursion part: n/2 사이즈의 nstance들 3번 recursion call
- Karatsuba 시간복잡도 함수는 O(n^1.58496)

&nbsp;

#### Fast Multiplication
- 1960: Kolmogorov -> 곱셈은 O(n^2)보다 빨리 안될거야
- 1962: Karatsuba -> 되는데?
  - 반 자르고 나면 3번만 재귀호출하면 돼 -> O(n^1.58496)
- 1963: Toom -> 반으로 자르지 말고 더 잘게 자르면 더 빠르게 됨
- 1966: Cook -> 더 간단하게 구현 가능
- 1971: Schönhage–Strassen -> FFT라는게 있는데 그걸 적용해 봄
- 2007: Furer -> 30년 넘게 걸렸지만 더 빠르게 됨
- 2019: Harvey & van der Hoeven -> O(n log n) 시간에 된다

D&C 하는 법
- 일단 점화식을 찾는다
- 그리고 푼다
  - Master Theorem, recursion tree 등

&nbsp;

# Dynamic Programming

Fibonacci Number
- Recursive definition of Fibonacci Numbers

- Recursive algorithm for Fibonacci Numbers

&nbsp;

#### RecFibo is Horribly Slow

```python
def RecFibo(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return RecFibo(n - 1) + RecFibo(n - 2)
```

RecFibo의 시간 복잡도  
T(n) = T(n - 1) + T(n - 2) + 1  
T(n) > Fn  

&nbsp;

#### Dynamic Programming: Fill the Table

- 재귀를 쓰는데 중복하지 말자
- Recursion to iteration
- Filling the table(array) in order

```
IterFibo2(n):  
prev <- 1  
curr <- 0  
for i <- i to n  
  next <- curr + prev  
  prev <- curr  
  curr <- next   
return curr  
```

&nbsp;

#### The Pattern: Dynamic Programming

Dynamic Programming is recursion without repetition
- DP 알고리즘은 중간의 subproblem에 대한 solution을 저장하는데
- 보통 array 혹은 table(2차원, 3차원, 혹은 그 이상)을 이용
- 정말 중요한 것은 올바른 점화식을 찾는 것
- 점화식을 이용하여 table을 순서대로 채워 나간다

> Dynamic programming is not about filling in tables   
it's about smart recursion

- DP를 사용하여 문제를 해결하는 일반적인 과정
  1. Formulate the problem recursively
    - 문제를 해결하는 recursive algorithm 혹은 점화식을 찾아내야함
    - 계산할 값이 무엇인지(what) 결정하고, 그것에 대한 점화식을 구함

  2. Build Solutions to your recurrence from the bottom up
    - Base case의 경우부터 답을 차곡차곡 table에 쌓아가는 방식으로 설계

점화식을 찾는 1단계가 가장 어려움

&nbsp;

문제
- 최단경로는 몇가지?
- A에서 B까지 가는 최단경로는 총 몇 가지일까요?

DP로 풀기 위해
- 우리가 손으로 계산하던 값들이 무엇인가?
- 그 값들은 어떻게 계산하는가?

계산할 값들의 정의 및 부분문제 설정
- P(i, j)를 (0, 0)에서 (i, j)까지 가는 최단경로 개수라 하자

계산할 값들에 대한 점화식 구하기
- 먼저 base case 알아내기
- 일반적 경우에 대한 점화식 찾기

Bottom-up 순으로 계산하는 알고리즘 설계 및 기술
- i 및 j 증가 순으로 P(i, j)을 계산해 나가면서 저장하면 됨
- O(MN) 시간
- 마지막으로 p(m, n)을 리턴

&nbsp;

#### Longest Common Subsequence
- 공통으로 나타나는 수열중에 제일 긴 것

A와 B는 배열형식으로 주어진다고 하자
A[1..m], B[1..n]

L(i, j)을 A[1..i]와 B[1..j] 사이의 LCS의 길이로 정의
- 우리가 원하는 답은 L(m, n)
- 나머지 i,j에 대한 L(i,j) 값들을 푸는 것이 부분문제(0<=i<=m-1, 0<=j<=n-1)
- Base case: i = 0 혹은 j = 0일때
- 일반적인 경우에 대한 L(i, j)의 재귀적 구조를 찾아라 -> 점화식