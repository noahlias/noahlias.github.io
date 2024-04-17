---
title: "Multi-threading and Multi-process"
date: 2024-04-17T19:00:43+0800
tags: ["CS"]

categories: ["Thinking"]
---

# 问题

这个东西的探讨要从前天在[b 站](https://www.bilibili.com/video/BV1ez421C7Fz)看到一个有趣的问题开始,
它启发了我对多进程与多线程的思考.

> 这个问题是: 有 2 个线程, 一个线程打印一个*A*, 一个线程打印*B*, 问如何让这两个线程交替打印 100 次?

## 线程

线程是操作系统能够进行运算调度的最小单位. 它被包含在进程之中, 是进程中的实际运作单位.
我第一时间看到了,用 condition 来处理线程的切换以及同步问题

### C

```c
#include <pthread.h>
#include <stdio.h>

#define NUM_ITERATIONS 100

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;
int print_a = 1;

void *print_a_thread(void *arg) {
  for (int i = 0; i < NUM_ITERATIONS; ++i) {
    pthread_mutex_lock(&mutex);
    while (!print_a) {
      pthread_cond_wait(&cond, &mutex);
    }
    printf("A\n");
    print_a = 0;
    pthread_cond_signal(&cond);
    pthread_mutex_unlock(&mutex);
  }
  return NULL;
}

void *print_b_thread(void *arg) {
  for (int i = 0; i < NUM_ITERATIONS; ++i) {
    pthread_mutex_lock(&mutex);
    while (print_a) {
      pthread_cond_wait(&cond, &mutex);
    }
    printf("B\n");
    print_a = 1;
    pthread_cond_signal(&cond);
    pthread_mutex_unlock(&mutex);
  }
  return NULL;
}

int main() {
  pthread_t thread_a, thread_b;

  // Create the threads
  pthread_create(&thread_a, NULL, print_a_thread, NULL);
  pthread_create(&thread_b, NULL, print_b_thread, NULL);

  // Wait for the threads to finish
  pthread_join(thread_a, NULL);
  pthread_join(thread_b, NULL);

  // Clean up
  pthread_mutex_destroy(&mutex);
  pthread_cond_destroy(&cond);

  return 0;
}
```

### Python

我就在思考, 这个问题如果用*python*来解决, 会是怎样的呢?

```python

import threading

NUM_ITERATIONS = 100

# Create a lock and an event
event_a = threading.Event()
event_a.set()
# Set the event to True, so that the first thread can start printing 'A'

event_b = threading.Event()
def print_a_thread():
    for _ in range(NUM_ITERATIONS):
            # Wait for the event to be set (which means it's 'A's turn)
            event_a.wait()
            print("A")
            # Clear the event to signal that it's not 'A's turn anymore
            event_a.clear()
            # Set the event for 'B' to start printing
            event_b.set()

def print_b_thread():
    for _ in range(NUM_ITERATIONS):
            # Wait for the event to be cleared (which means it's 'B's turn)
            event_b.wait()
            print("B")
            # Set the event to signal that it's not 'B's turn anymore
            event_b.clear()
            # Set the event for 'A' to start printing
            event_a.set()

# Create the threads
thread_a = threading.Thread(target=print_a_thread)
thread_b = threading.Thread(target=print_b_thread)

# Start the threads
thread_a.start()
thread_b.start()

# Wait for the threads to finish
thread_a.join()
thread_b.join()

```

### Golang

因为好奇,对协程友好的**golang**会怎么解决这个问题呢?

```go
package main

import (
	"fmt"
	"sync"
)

const NUM_ITERATIONS = 100

func printA(wg *sync.WaitGroup, chA, chB chan struct{}) {
	defer wg.Done()
	for i := 0; i < NUM_ITERATIONS; i++ {
		<-chA
		fmt.Println("A")
		chB <-struct{}{} // Wait for the other goroutine to print 'B'
	}
}

func printB(wg *sync.WaitGroup, chA,chB chan struct{}) {
	defer wg.Done()
	for i := 0; i < NUM_ITERATIONS; i++ {
		<-chB // Wait for the other goroutine to print 'A'
		fmt.Println("B")
		if i < NUM_ITERATIONS-1 {
			chA<-struct{}{} // Signal the other goroutine to print 'A'
		}
	}
}

func main() {
	var wg sync.WaitGroup
	chA := make(chan struct{},1)
	chB := make(chan struct{})

	wg.Add(2)
	go printA(&wg, chA, chB)
	go printB(&wg, chA, chB)
	chA <- struct{}{} // Start the first goroutine

	wg.Wait() // Wait for both goroutines to finish
	close(chA)
	close(chB)
}
```

Golang 是真的很简洁了, 用 channel 来解决这个问题,太简洁了

## 进程

进程是操作系统资源分配的最小单位, 它是程序的一次执行过程, 是一个动态的概念, 是程序在一个数据集合上的一次运行活动.
我最近看[jyy](https://jyywiki.cn/OS/2024/lect14.md)的操作系统课程,我又重新理解了`fork`/`execve`/`_wait` 等系统调用, 以及进程间通信的方式.

```c
#include <stdio.h>
#include <unistd.h>

int main(){

  for (int i = 0; i < 3; i++){
    fork();
    printf("Hello");
    printf("\n");
    //fflush(stdout);
  }
}

```

如何理解这个进程会打印**14**个*Hello*呢?

$ 2 + 4+ 8 = 14 $

fork 是一个二叉树的过程, 一个进程 fork 出来的子进程, 会继续 fork 出来一个子进程, 以此类推, 所以会有 14 个*Hello*.
记住它是一个状态机的复制

### Fork Bomb

你知道什么是 fork 炸弹吗? 一个简单的 shell 脚本就可以让你的系统崩溃

```shell
:(){ :|:& };:
```

上面的命令等同于

```shell
bomb() {
  bomb | bomb &
}
```

## 总结

如果我想知道更多,我觉得我未来需要去了解硬件实现,需要知道每一个原子操作是怎么实现的,这样我才能更好的理解操作系统,编程语言,编译器等等.
