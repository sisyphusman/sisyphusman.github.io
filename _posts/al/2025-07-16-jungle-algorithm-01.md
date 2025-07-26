---
layout: post
title : 정글 알고리즘 1강
category: al
---

![Structure and Interpretation of Computer Programs](assets/images/al/SICP.jpg)

Structure and Interpretation of Computer Programs (컴퓨터 프로그램의 구조와 해석)   
내용 중 일부

&nbsp;

#### 프로그래밍의 요소(Element of Programming)

기본 표현식 (primitivie expression) | 언어가 다루는 가장 단순한 요소 (which represent the simplest entities the language is concerned with)
결합 수단 (means of combination) | 단순한 요소들을 결합하여 복합적인 요소를 만드는 방법 (by which compound elements are built from simpler ones, and)
추상화 수단 (means of abstraction) | 복합 요소에 이름을 붙이고, 그것을 하나의 단위로써 다룰 수 있게 해주는 방법 (by which compound elements can be named and manipulated as units)

**추상화**가 무엇인가 -> 복합 요소의 이름을 지정하고 단위로 조작할 수 있는 것   
(means of abstraction, by which compound elements can be named and manipulated as units)

추상화를 해서 써야 한다 -> **재사용성**이 좋아짐

#### Substitution Model (치환 모델)

&nbsp;

Applicative Order Evaluation (C, C++, Java, Python 등)
> 인자를 먼저 계산한 뒤, 함수 본문에 치환하여 실행

```python
    def f(a):
        return sum_of_square(a+1, a*2)
    def sum_of_squares(x,y):
        return square(x) + square(y)
    def square(x):
        return x * x
```
&nbsp;

Normal Order Evaluation (Haskell이나 일부 LISP 시스템)
> 함수 본문에서 실제로 필요한 시점까지 인자 평가를 미룸

```python
    def p():
        while true:
            pass
    def test(x, y):
        return 0 if x == 0 else y 

    test(0, p())
```

test(0, p())에서 무한 루프로 빠지지 않고 0을 리턴 하는 이유 -> Normal Order Evaluation  

필요할때까지 매개 변수 계산을 미룸

&nbsp;

#### 재귀 (Recursion)

> recursion은 사람의 생각대로 자연스럽게 쓰는 방식

&nbsp;

0부터 10까지 더하는 함수를 재귀로 표현

```python
def nsum(n):
    if n == 0:
        return 0
    return n + nsum(n-1)

print(nsum(10))
```

&nbsp;

0부터 10까지 더하는 함수를 **꼬리 재귀**로 표현

```python
def exp_iter(n, total):
    if n == 0:
        return total
    else:
        return exp_iter(n-1, total+n)
    
def sum(n):
    return exp_iter(n, 0)
```

&nbsp;

#### 거듭제곱의 예시

$$
b^n =
\begin{cases}
1 & \text{if } n = 0 \\
b \times b^{n-1} & \text{if } n > 0
\end{cases}
$$

```python
# recursion version

def expt(b, n):
    if n == 0:
        return 1
    else:
        return b * expt(b, n - 1)
```

```python
# iteration version

def expt_iter(b, n):
    product = 1
    for i in range(n):
        product *= b
    
return product
```

```python
# tail recursion version

 def expt_iter(b, counter, product):
    if counter == 0:
        return product
    else:
        return expt_iter(b, counter - 1, b * product)

#product = 누적값 = 1
```

&nbsp;

- 꼬리 재귀(Tail Recursion)은 결과값을 누적하며 계산-> 더 빠르게 값을 알 수 있다

- 꼬리 재귀는 for문과 유사하다

- for문, 재귀, 꼬리 재귀에서 제일 오래 걸리는 것은 일반 재귀 함수이다 (스택으로 인한 오버헤드)

&nbsp;

#### Break the problem into smaller subproblems

> 재귀는 큰 문제를 작은 문제로 나누고, 그 작은 문제가 자기 자신과 구조가 동일할 때 적용할 수 
있는 해결 방법

- 문제 파악: 이 문제를 자기 자신보다 작은 하위 문제로 표현할 수 있는가?

- 기저 조건 정의: 언제 재귀 호출을 멈출 것인가?

- 하위 문제 호출: 더 작은 인풋에 대해 자기 자신을 호출

- 결과 조합: 하위 문제의 결과를 조합하여 전체 문제 해결