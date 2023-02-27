---
title: "Higher-Order Function"
date: 2023-02-27T22:27:00+08:00
draft: false
tags: ["Functional Programming","Lambda"]
categories: ["Cs61A"]
mathjax: true
---

## Lab02

### lambda_curry

匿名函数的柯理化

```python
def lambda_curry2(func):
    """
    Returns a Curried version of a two-argument function FUNC.
    >>> from operator import add, mul, mod
    >>> curried_add = lambda_curry2(add)
    >>> add_three = curried_add(3)
    >>> add_three(5)
    8
    >>> curried_mul = lambda_curry2(mul)
    >>> mul_5 = curried_mul(5)
    >>> mul_5(42)
    210
    >>> lambda_curry2(mod)(123)(10)
    3
    """
    return lambda x: lambda y: func(x,y)
```

### count_cond

计算1到`N` 中符合 情况函数的数字占比数

```python
def count_cond(condition):
    """Returns a function with one parameter N that counts all the numbers from
    1 to N that satisfy the two-argument predicate function Condition, where
    the first argument for Condition is N and the second argument is the
    number from 1 to N.

    >>> count_factors = count_cond(lambda n, i: n % i == 0)
    >>> count_factors(2)   # 1, 2
    2
    >>> count_factors(4)   # 1, 2, 4
    3
    >>> count_factors(12)  # 1, 2, 3, 4, 6, 12
    6

    >>> is_prime = lambda n, i: count_factors(i) == 2
    >>> count_primes = count_cond(is_prime)
    >>> count_primes(2)    # 2
    1
    >>> count_primes(3)    # 2, 3
    2
    >>> count_primes(4)    # 2, 3
    2
    >>> count_primes(5)    # 2, 3, 5
    3
    >>> count_primes(20)   # 2, 3, 5, 7, 11, 13, 17, 19
    8
    """
    return lambda n: sum(1 for i in range(1, n+1) if condition(n, i))
```

### composer

组合函数

```python
def composer(f, g):
    """Return the composition function which given x, computes f(g(x)).

    >>> add_one = lambda x: x + 1        # adds one to x
    >>> square = lambda x: x**2
    >>> a1 = composer(square, add_one)   # (x + 1)^2
    >>> a1(4)
    25
    >>> mul_three = lambda x: x * 3      # multiplies 3 to x
    >>> a2 = composer(mul_three, a1)    # ((x + 1)^2) * 3
    >>> a2(4)
    75
    >>> a2(5)
    108
    """
    return lambda x: f(g(x))


def composite_identity(f, g):
    """
    Return a function with one parameter x that returns True if f(g(x)) is
    equal to g(f(x)). You can assume the result of g(x) is a valid input for f
    and vice versa.

    >>> add_one = lambda x: x + 1        # adds one to x
    >>> square = lambda x: x**2
    >>> b1 = composite_identity(square, add_one)
    >>> b1(0)                            # (0 + 1)^2 == 0^2 + 1
    True
    >>> b1(4)                            # (4 + 1)^2 != 4^2 + 1
    False
    """

    return lambda x: f(g(x)) == g(f(x))
```

### cycle

根据描述可以分析得出以下几个情况:

- 当n在0、1、2、3的情况下
- 当n为3的倍数下 判断余数选择不同的函数进行组合等

```python
def cycle(f1, f2, f3):
    """Returns a function that is itself a higher-order function.

    >>> def add1(x):
    ...     return x + 1
    >>> def times2(x):
    ...     return x * 2
    >>> def add3(x):
    ...     return x + 3
    >>> my_cycle = cycle(add1, times2, add3)
    >>> identity = my_cycle(0)
    >>> identity(5)
    5
    >>> add_one_then_double = my_cycle(2)
    >>> add_one_then_double(1)
    4
    >>> do_all_functions = my_cycle(3)
    >>> do_all_functions(2)
    9
    >>> do_more_than_a_cycle = my_cycle(4)
    >>> do_more_than_a_cycle(2)
    10
    >>> do_two_cycles = my_cycle(6)
    >>> do_two_cycles(1)
    19
    """
    def helper(n):
        if n == 0:
            return lambda x: x
        if n == 1:
            return f1
        if n == 2:
            return composer(f2, f1)
        if n == 3:
            return composer(f3, composer(f2, f1))
        g = helper(0)
        f = helper(n%3)
        tmp = n //3
        while tmp > 0:
            g  =  composer(helper(3),g)
            tmp -=1
        return composer(f, g)
    return helper
```

## Summary

### c++ lambda

```cpp
    auto a = [](int a ){ return a+1;};
```

### rust lambda

```rust
    let a = |x,y| x+y;
```

这部分 让我想起来不同语言中的**lambda**, 正好最近在学习**C++** 和**Rust**,
复习了一下不同语言的lambda函数,虽然`CS61A`中
里面有**SCHEME**这门FP语言, 让我认识到不同函数式编程语言的共性 和相同的特点等

- 函数是第一公民
- 纯函数(无副作用)

## Reference

- [CS61A](https://cs61a.org/lab/lab02/)
- [Functional Programming](https://en.wikipedia.org/wiki/Functional_programming)