---
title: "位运算"
date: 2022-12-01T11:07:03+08:00
draft: false
tags: ["bit_operations"]
categories: ["CS61C"]
---

## 描述

```c
#include <stdio.h>
#include "bit_ops.h"

/* Returns the Nth bit of X. Assumes 0 <= N <= 31. */
unsigned get_bit(unsigned x, unsigned n)
{
    return (x >> n) & 1; /* UPDATE WITH THE CORRECT RETURN VALUE*/
}

/* Set the nth bit of the value of x to v. Assumes 0 <= N <= 31, and V is 0 or 1 */
void set_bit(unsigned *x, unsigned n, unsigned v)
{
    if (v)
    {
        *x |= 1 << n;
    }
    else
    {
        *x &= (~(1 << n));
    }
}

/* Flips the Nth bit in X. Assumes 0 <= N <= 31.*/
void flip_bit(unsigned *x, unsigned n)
{
    *x ^= (1 << n);
}
```

测试案例

```c
#include <stdio.h>
#include "bit_ops.h"

void test_get_bit(unsigned x, unsigned n, unsigned expected) {
    unsigned a = get_bit(x, n);
    if(a!=expected) {
        printf("get_bit(0x%08x,%u) returned 0x%08x, but we expected 0x%08x\n",x,n,a,expected);
    } else {
        printf("get_bit(0x%08x,%u)returned 0x%08x, correct\n",x,n,a);
    }
}

void test_set_bit(unsigned x, unsigned n, unsigned v, unsigned expected) {
    unsigned o = x;
    set_bit(&x, n, v);
    if(x!=expected) {
        printf("set_bit(0x%08x,%u,%u) returned 0x%08x but we expected 0x%08x\n",o,n,v,x,expected);
    } else {
        printf("set_bit(0x%08x,%u,%u) returned 0x%08x, correct\n",o,n,v,x);
    }
}

void test_flip_bit(unsigned x, unsigned n, unsigned expected) {
    unsigned o = x;
    flip_bit(&x, n);
    if(x!=expected) {
        printf("flip_bit(0x%08x,%u) returned 0x%08x, but we expected 0x%08x\n",o,n,x,expected);
    } else {
        printf("flip_bit(0x%08x,%u) returned 0x%08x, correct\n",o,n,x);
    }
}

int main(int argc, const char * argv[]) {
    printf("\nTesting get_bit()\n\n");
    test_get_bit(0b1001110,0,0);
    test_get_bit(0b1001110,1,1);
    test_get_bit(0b1001110,5,0);
    test_get_bit(0b11011,3,1);
    test_get_bit(0b11011,2,0);
    test_get_bit(0b11011,9,0);
    printf("\nTesting set_bit()\n\n");
    test_set_bit(0b1001110,2,0,0b1001010);
    test_set_bit(0b1101101,0,0,0b1101100);
    test_set_bit(0b1001110,2,1,0b1001110);
    test_set_bit(0b1101101,0,1,0b1101101);
    test_set_bit(0b1001110,9,0,0b1001110);
    test_set_bit(0b1101101,4,0,0b1101101);
    test_set_bit(0b1001110,9,1,0b1001001110);
    test_set_bit(0b1101101,7,1,0b11101101);
    printf("\nTesting flip_bit()\n\n");
    test_flip_bit(0b1001110,0,0b1001111);
    test_flip_bit(0b1001110,1,0b1001100);
    test_flip_bit(0b1001110,2,0b1001010);
    test_flip_bit(0b1001110,5,0b1101110);
    test_flip_bit(0b1001110,9,0b1001001110);
    printf("\n");
    return 0;
}
```

## 总结

- 将x最右边的n位清零：x & (~ 0<<n)
- 获取x的第n位值（0或1）：(x >> n) & 1
- 获取x的第n位幂值：x & (1 << (n-1))
- 仅将第n位置为1：x | (1<<n)
- 仅将第n位置为0：x & (~(1<<n))
- 将x最高位至第n位（含）清零：x & ((1 << n)-1)
- 将x第n位至第0位（含）清零：x & (~((1 << (n+1))-
