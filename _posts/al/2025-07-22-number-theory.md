---
layout: post
title: 정수론(Number Theory) [임시]
category: al
---

# 정수론
```python
# 최대공약수 (GCD) – 유클리드 호제법, r = a % b를 이용해 GCD(a, b) == GCD(b, r)가 성립
def gcd(a, b):
    return a if b == 0 else gcd(b, a % b)
```