---
layout: post
title: 재귀(Recursion)
category: al
---

# 재귀 함수

  ![재귀 구글 검색](/assets/images/al/recursion-01.gif)
  
  구글에서 recursion을 검색하면 뜨는 화면인데 버튼을 눌러도 같은 결과가 반복된다
  
  재귀 호출처럼 끝없이 반복되는 유머이다
  
  &nbsp;
  
  ```python
  def fact(n):
      if n <= 1:
          return 1
      return n * fact(n - 1)
  ```
  
  간단한 팩토리얼 재귀 함수이다
  
  fact(5)일때 5 * 4 * 3 * 2 * 1 = 120이 나온다
  
  재귀는 **절차지향적 사고**에서 벗어나서 **귀납적 사고**를 해야 한다
  
  현재 문제를 해결하기보다 더 작은 문제로 나눠서, 결국 가장 **단순한 형태**(base case or base condition)에 도달하도록 유도하는 사고방식이다.
  
  중요한 것은 항상 **종료 조건**이 있어야 무한 루프에 빠지지 않는다
  
  함수의 인자를 어떻게 **설계**할 것인지 명확히 정해야 한다
  
  모든 재귀 함수는 반복문(iteration)으로 만들 수 있다

  하지만 반복문으로 만들면 더 복잡한 경우도 많다
  
  반복문보다 재귀가 코드가 간결하지만, 메모리/시간에서는 **손해**를 본다
  
  &nbsp;
  
  ```python
  def fib(n):
      if n <= 1:
          return n
      return fib(n - 1) + fib(n - 2)
  ```
  
  
  피보나치 수열의 경우 재귀로 풀때 $$ O(1.618^n) $$ 이다 n = 100일때 엄청난 시간이 걸리게 된다
  
  한번 계산한 부분을 다시 계산하기 때문에 매우 비효율적인 방법이 되어버린다

  메모이제이션(memoization) 또는 반복문을 이용하는 것이 바람직하다


  &nbsp;

  ---

  ```python
  def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
  ```

  - 선형 재귀(Linear Recursion)  
  한 번에 재귀 호출이 하나만 발생

  &nbsp;

  ```python
  def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)
  ```
  - 이진 재귀(Binary Recursion)  
  한 번에 두 개의 재귀 호출이 발생, 보통 트리 형태로 분기됨

  &nbsp;

  ```python
  def mystery(n):
    if n <= 0:
        return
    mystery(n - 1)
    mystery(n - 2)
    mystery(n - 3)
  ```

  - 다중 재귀(Multiple Recursion)  
  이진을 넘어서 여러 개의 재귀 호출이 발생

  &nbsp;

  ```python
  def tail_fact(n, acc=1):
    if n <= 1:
        return acc
    return tail_fact(n - 1, acc * n)
  ```

  - 꼬리 재귀(Tail Recursion)  
  반환할때 더 이상 계산이 필요 없음 -> 최적화 가능

  &nbsp;

  ```python
  def permute(arr, path):
    if not arr:
        print(path)
        return
    for i in range(len(arr)):
        permute(arr[:i] + arr[i+1:], path + [arr[i]])
  ```
  - 트리 재귀(Tree Recursion)  
  이진과 유사하지만, 구조적으로 트리 탐색처럼 생긴 재귀

  &nbsp;

  ```python
  def dfs(graph, node, visited):
    if node in visited:
        return
    visited.add(node)
    for neighbor in graph[node]:
        dfs(graph, neighbor, visited)
  ```
  - 그래프 재귀(Graph-style Recursion)  
  무한 루프나 중복 방문을 방지하기 위해 방문 체크(visited)가 필요