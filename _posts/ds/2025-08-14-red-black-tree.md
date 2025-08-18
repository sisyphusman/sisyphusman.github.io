---
layout: post
title: 레드 블랙 트리(Red Black Tree)
category: ds
---

![레드블랙트리](/assets/images/ds/Red-black_tree_1.png)

- 이진 탐색 트리(BST)의 한 종류

- 스스로 균형 잡는 트리

- BST의 worst case의 단점을 개선
  - 일반 BST는 최악의 케이스일때 O(n)
  - 레드 블랙 트리는 최악의 케이스를 항상 O(log n)

- 모든 노드는 red 혹은 black

- 루트 노드는 black

nil 노드란?

- 존재하지 않음을 의미하는 노드

- 자녀가 없을 때 자녀를 nil 노드로 표기

- 값이 있는 노드와 동등하게 취급

- RB 트리에서 leaf 노드는 nil 노드

- 모든 nil 노드는 black

Red-Black 트리 속성
  1. 모든 노드는 red or black
  2. 루트 노드는 black
  3. 모든 nil 노드는 black
  4. 노드가 red라면 자녀들은 black
  5. 각 노드에서 자손 nil 노드들까지 가는 모든 경로는 black 수와 같다

노드 x의 black height 

- 노드 x에서 임의의 자손 nil 노드까지 내려가는 경로에서 black 수 (자기 자신은 카운트에서 제외)

- RB 트리가 두 자녀가 같은 색을 가질 때 
부모와 두 자녀의 색을 바꿔줘도 임의의 노드에서 자손 nil 노드들까지 가는 경로들의 black 수는 같다

RB 트리의 군형 잡는 법

- 삽입/삭제 시 노드가 red라면 
1. 자녀들은 black  
2. 임의의 노드에서 자손 nil 노드들까지 가는 경로들의 black 수는 같게 하려고 구조를 바꾸다 보면 트리의 군형이 잡히게 된다

RB 트리와 Sentinal
- nil 노드를 사용하는 것이 Sentinal
- 혹은 null을 사용

#### 삽입

- 마지막에 root node를 black으로 고정

- 삽입 하는 노드가 red인 이유는 nil 노드까지 가는 경로의 black 수에 영향을 미치지 않기 때문

- Case 1: 삼촌 RED → 재색 + 위로 올리기

  Case 2: 삼촌 BLACK + 꺽임(삼각형) → 작은 회전으로 바깥(직선) 만들기

  Case 3: 삼촌 BLACK + 바깥(직선) → 큰 회전 + 재색 
