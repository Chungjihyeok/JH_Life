---
layout: post
title: 밑바닥부터 시작하는 딥러닝
date: 2019-11-03
excerpt: "'Deep Learning from Scratch - 밑바닥부터 시작하는 딥러닝 -'책 정리하기"
tag:
- python
- deep learning
---


# Chater 2 "Perceptron" 


## AND Gate Python으로 구현하기 

```python
# 내가 만든 
def and1(x1:int, x2:int)->int:
    return x1*x2

def and3(x1:int, x2:int)->int:
    x = np.array([x1, x2])
    w = np.array([0.5, 0.5])
    b = -0.7
    temp = np.sum(x*w) + b
    return (lambda x:1 if 1/(1 + np.exp(-x)) > 0.5 else 0)(temp)
    
# 책 내용 변형 
def and2(x1:int, x2:int)->int:
    w1, w2, threshold = 0.5, 0.5, 0.7
    temp = x1*w1 + x2*w2 
    if temp > threshold:
        return 1
    else:
        return 0
        
%%timeit
and1(0,0)
138 ns ± 11.7 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%%timeit 
and2(0,0)
413 ns ± 53 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%%timeit
and3(0,1)
12.8 µs ± 389 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

<br>

## OR Gate Python으로 구현하기 

```python
# 내가 만든  
def or1(x1:int, x2:int)->int:
    return (lambda x1, x2: 1 if x1+x2 > 0 else 0)(x1, x2)
    
def or4(x1:int, x2:int)->int:
    x = np.array([x1, x2])
    w = np.array([0.5, 0.5])
    b = -0.1
    temp = np.sum(w*x) + b
    return (lambda x: 1 if 1/(1 + np.exp(-x)) > 0.5 else 0)(temp)
    
# 책 내용 변형
def or2(x1:int, x2:int)->int:
    w1, w2, threshold = 0.5, 0.5, 0.2
    temp = x1*w1 + x2*w2 
    if temp > threshold:
        return 1
    else:
        return 0

import numpy as np

def or3(x1:int, x2:int)->int:
    x = np.array([x1, x2])
    w = np.array([0.5, 0.5])
    b = -0.2
    temp = np.sum(w*x) + b
    if temp > 0:
        return 1
    else:
        return 0
        
%%timeit
or1(0,0)      
385 ns ± 68.5 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%%timeit
or1(0,1)
418 ns ± 43.6 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%%timeit
or2(0,0)
501 ns ± 40 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%%timeit
or2(0,1)
379 ns ± 43.4 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%%timeit
or3(0,0)
9.47 µs ± 583 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

%%timeit
or3(0,1)
9.74 µs ± 588 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

%%timeit
or4(0,0)
13.9 µs ± 1.57 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)
```

<br>

## XOR Gate Python으로 구현하기 

```python
def and1(x1:int, x2:int)->int:
    x = np.array([x1, x2])
    w = np.array([0.5, 0.5])
    b = -0.7
    temp = np.sum(x*w) + b
    return (lambda x:1 if 1/(1 + np.exp(-x)) > 0.5 else 0)(temp)

def or1(x1:int, x2:int)->int:
    x = np.array([x1, x2])
    w = np.array([0.5, 0.5])
    b = -0.1
    temp = np.sum(w*x) + b
    return (lambda x: 1 if 1/(1 + np.exp(-x)) > 0.5 else 0)(temp)

def nand(x1:int, x2:int)->int:
    x = np.array([x1, x2])
    w = np.array([-0.5, -0.5])
    b = 0.7
    temp = np.sum(x*w) + b
    return (lambda x:1 if 1/(1 + np.exp(-x)) > 0.5 else 0)(temp)
    
def xor(x1:int, x2:int)->int:
    s1 = nand(x1,x2)
    s2 = or1(x1,x2)
    result = and1(s1,s2)
    return result

%%timeit
xor(0,1)
40.7 µs ± 4.55 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)
```