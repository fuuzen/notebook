---
title: 线程
date: 2024/02/23
math: true
categories:
  - [计算机科学, 并行程序设计]
tags:
  - 并行
---

## pthread 库

### 简介

pthread 是 POSIX 线程库,它是一种跨平台的线程库,可以在不同的操作系统上使用。它是 C 语言的标准库,提供了一组函数来创建、管理和同步线程。"pthread"库相对于"thread"库来说,更加底层和与操作系统紧密相关。

!!! info 
    thread 是 C++11 标准提供的线程库,它是 C++ 的一部分。它提供了一种面向对象的方式来创建、管理和同步线程。"thread"库提供了 std::thread 类,可以用于创建和管理线程,并提供了一些成员函数和工具函数来操作线程。

pthread 相关函数需要引入标准库头文件：`#include<pthread.h>`

### 参考资料

开源教程：https://hpc-tutorials.llnl.gov/posix/

pthread 库在不同的操作系统上有不同的实现，因此，官方的 pthread 库文档可能由各个操作系统的文档或开发者文档提供。以下是一些常见操作系统的 pthread 官方文档链接：

- Linux: 在 Linux 系统上，pthread 库的官方文档可以在 GNU C 库（glibc）的文档中找到。您可以访问以下链接获取详细的 pthread 文档：https://www.gnu.org/software/libc/manual/html_node/Threads.html

- macOS: 在 macOS 上，pthread 库的官方文档可以在 Apple 开发者文档中找到。您可以访问以下链接获取有关 pthread 的详细信息：https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/Introduction/Introduction.html

- Windows: 在 Windows 操作系统上，Microsoft 提供了一种名为"Windows Threads"的线程库，它是 Windows 对 POSIX 线程的实现。您可以在 Microsoft 的官方文档中找到有关"Windows Threads"的详细信息：https://docs.microsoft.com/en-us/windows/win32/procthread/creating-threads

- IBM:

  - z/OS:

    z/OS V2R4 POSIX Threads Programming: https://www.ibm.com/docs/en/zos/3.1.0?topic=programming-posix-threads-programming

  - IBM i:

    IBM i 7.4 PASE Programming and Reference: https://www.ibm.com/docs/en/i/7.4?topic=programming-pase-programming-reference

  - AIX:

    AIX 7.2 Library Reference - Threads and Concurrency: https://www.ibm.com/docs/en/aix/7.2?topic=library-threads-concurrency

不同操作系统上的 pthread 库在语法和使用方式上可能会有一些差异。这是因为 pthread 库是根据 POSIX 线程标准实现的，而各个操作系统可能会在标准的基础上进行一些定制和扩展。

## 多线程程序举例分析

### 线程间接执行的函数

```c
float *func1(float a, float b) {
    int i;
    float *result;
    result = (float *)malloc(sizeof(float) * 2);
    for (i = 0; i < 1000000000; ++i) {
        result[0] = a + b;
        result[1] = a - b;
    }
    return result;
}

float *func2(float a, float b) {
    int i;
    float *result;
    result = (float *)malloc(sizeof(float) * 2);
    for (i = 0; i < 1000000000; ++i) {
        result[0] = a * b;
        result[1] = a / b;
    }
    return result;
}
```

### 给间接函数传递参数用的结构体

传递给子线程的将是这个结构体变量的**地址**，这个地址对父线程和子线程都是一样的，可见的，所以其内容是共享的，互相可以看到对方的修改。

```c
typedef struct {
    float a;
    float b;
} Args;
```

### 线程直接执行的函数

只能返回 void 指针类型，即返回 `void*`

`return` 返回的地址内容将被 `pthread_join` 读出

```c
void *thread_func1(void *arg) {
    Args *args = (Args *)arg; // 传递参数结构体变量的地址
    return func1(args->a, args->b);
}

void *thread_func2(void *arg) {
    Args *args = (Args *)arg; // 传递参数结构体变量的地址
    return func2(args->a, args->b);
}
```

#### pthread_exit 结束线程

除了 `return` 返回数据，还可以用 `pthread_exit` 函数返回数据并直接结束线程，而不必等到主线程`pthread_join` 连接该线程。提前结束进程可以释放进程占用的内存、处理器，节省计算机资源 (真的能吗?)

线程终止函数 `pthread_exit` 传入 1 个参数

- `void *` 类型：非 void 类型指针，指向返回数据的地址

```c
void *thread_func1(void *arg) {
    Args *args = (Args *)arg;
    pthread_exit(func1(args->a, args->b));
}

void *thread_func2(void *arg) {
    Args *args = (Args *)arg;
    pthread_exit(func1(args->a, args->b));
}
```

### 线程返回数据存储的有类型地址

应当是数据的类型

```c
result1 = (float *)malloc(sizeof(float) * 2);
result2 = (float *)malloc(sizeof(float) * 2);
```

### 线程返回数据直接存储的无类型地址

只能是 void 指针类型

```c
void *r1, *r2;
```

### 定义 pthread_t 类型变量

`pthread_t` 类型变量用于存储线程句柄 (TID) ，`t`表示数据类型 ( type )

```c
pthread_t tid1, tid2;
```

### pthread_create 创建线程

线程创建函数 `pthread_create` 传入 4 个参数：

- `pthread_t`类型：新线程的线程句柄(TID)要存储的地址(pthread_t 变量指针)
- `const pthread_attr_t *`类型：属性
- 执行函数 (只能是 void 类型，即无返回)
- 传给执行函数的参数 (一次只能传一个参数，所以一般用结构体传参数)

```c
pthread_create(&tid1, NULL, thread_func1, &ab);
pthread_create(&tid2, NULL, thread_func2, &ab);
```

函数返回结果：代表线程连接的状态，成功返回 0 ，否则返回错误码

错误码：

- EAGAIN : The system lacks the necessary resources to create another thread.
- EINVAL : The value specified by _thread_ is null.
- ELEMULTITHREADFORK : pthread_create() was invoked from a child process created by calling fork() from a multi-threaded process. This child process is restricted from becoming multi-threaded.
- ENOMEM : There is not enough memory to create the thread.

#### :warning:区别进程的创建

```c
for(i = 0; i < 5; i++)
    pthread_join(tid[i], NULL);
```

这里与 Linux 创建进程（线程）的 `fork` 不同，后者会创建 $2^5$ 个进程（线程），而这里是 5 个线程！因为子线程、父线程是并行的！

### pthread_join 连接并读出线程执行结果

线程数据读出函数 `pthread_join` 传入 2 个参数：

- `pthread_t`类型：线程句柄，即 TID
- `void **`类型：执行函数的返回数据要存储的地址 (地址只能是 void 类型，一次只能返回一个地址)

```c
pthread_join(tid1, &r1);
pthread_join(tid2, &r2);
```

函数返回结果：代表线程连接的状态，成功返回 0 ，否则返回错误码

错误码：

- EDEADLK : A deadlock was detected (e.g., two threads tried to join with each other); or thread specifies the calling thread.
- EINVAL : hread is not a joinable thread.
- EINVAL : Another thread is already waiting to join with this thread.
- ESRCH : No thread with the ID thread could be found.

!!! warning
    一个线程只能同时连接一个线程，子线程自创建开始执行，若连接子线程时子线程还没结束，主线程会等到子线程执行结束后再读出数据

!!! abstract
    Windows 中使用的是 `WaitForSingleObject(... , ...)` / `WaitForMultipleObject(... , ...)` ，因为 Windows 将所有资源对象命名为 Object

### 无类型地址数据存储到有类型地址

```c
result1 = (float *)r1;
result2 = (float *)r2;
```

### 完整程序

```c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>

float *func1(float a, float b) {
    int i;
    float *result;
    result = (float *)malloc(sizeof(float) * 2);
    for (i = 0; i < 1000000000; ++i) {
        result[0] = a + b;
        result[1] = a - b;
    }
    return result;
}

float *func2(float a, float b) {
    int i;
    float *result;
    result = (float *)malloc(sizeof(float) * 2);
    for (i = 0; i < 1000000000; ++i) {
        result[0] = a * b;
        result[1] = a / b;
    }
    return result;
}

typedef struct {
    float a;
    float b;
} Args;

void *thread_func1(void *arg) {
    Args *args = (Args *)arg;
    return func1(args->a, args->b);
}

void *thread_func2(void *arg) {
    Args *args = (Args *)arg;
    return func1(args->a, args->b);
}

int main() {
    float a = 1.14, b = 5.14, *result1, *result2, during;
    Args ab;
    ab.a = a;
    ab.b = b;
    result1 = (float *)malloc(sizeof(float) * 2);
    result2 = (float *)malloc(sizeof(float) * 2);
    struct timeval start, end;
    gettimeofday(&start, NULL);
    void *r1, *r2;
    pthread_t tid1, tid2;
    pthread_create(&tid1, NULL, thread_func1, &ab);
    pthread_create(&tid2, NULL, thread_func2, &ab);
    pthread_join(tid1, &r1);
    pthread_join(tid2, &r2);
    result1 = (float *)r1;
    result2 = (float *)r2;
    gettimeofday(&end, NULL);
    during = (end.tv_sec - start.tv_sec) + (end.tv_usec - start.tv_usec) / 1000000.0;
    printf("result1[0] = %f\nresult1[1] = %f\n", result1[0], result1[1]);
    printf("result2[0] = %f\nresult2[1] = %f\n", result2[0], result2[1]);
    printf("during time = %f s\n", during);
    free(result1);
    free(result2);
    return 0;
}
```

## 线程间互动

### 互斥锁 pthread_mutex

对应数据类型 ( type ) 为 `pthred_mutex_t`，以及相关函数 `pthread_mutex_function`

互斥锁的作用主要是每个线程都可以在进入某个区域时 把这个区域锁起来， 不让其他的线程进入。 当自己的工作完成时或者达成某种条件时，则打开互斥锁。互斥锁存在的目的就是为了避免很多线程同时进入一个事件时发生冲突。

### pthread_cond 条件变量

对应数据类型 ( type ) 为 `pthred_cond_t`，以及相关函数 `pthread_cond_function`

条件变量主要和互斥锁一同使用，也就是当满足某一个条件时打开互斥锁。这个条件可以通过`pthread_cond_signal` 或者是`pthread_cond_broadcast` 发出

#### `pthread_cond_signal`

- 一次只能激活一个条件变量
- 不容易出现“线程打架”

#### `pthread_cond_broadcast`

- 一次性激活所有等待的条件变量
- 容易出现“线程打架”，造成“死锁”

### 线程互动程序举例分析

#### 初始化锁 `pthread_mutex_t`

```c
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
```

#### 初始化条件变量 `pthread_cond_t`

```c
pthread_cond_t cond_catchfish_finish = PTHREAD_COND_INITIALIZER;
pthread_cond_t cond_eatfish_finish = PTHREAD_COND_INITIALIZER;
```

#### 线程函数

#### `pthread_mutex_lock` 上锁

```c
pthread_mutex_lock (&mutex) ;
```

上锁之后线程将无法收到来自其他线程的信号

##### 输入

```c
int pthread_mutex_lock(pthread_mutex_t *mutex);

//SUSv3:
int pthread_mutex_lock(pthread_mutex_t *mutex);
```

##### 返回值

- If successful, pthread_mutex_lock() returns 0.

- If unsuccessful, pthread_mutex_lock() returns -1 and sets errno to one of the following values:

  - **Error Code**

    - \*Description\*\*

  - EAGAIN

    - The mutex could not be acquired because the maximum number of recursive locks for mutex has been exceeded. This errno will only occur in the shared path.

  - EDEADLK

    - The current thread already owns the mutex, and the mutex has a _kind_ attribute of \_\_MUTEX_NONRECURSIVE.

  - EINVAL
    - The value specified by _mutex_ is not valid.

**Special behavior for Single UNIX Specification, Version 3:** If unsuccessful, pthread_mutex_lock() returns an error number to indicate the error. SUSv3 操作系统可以直接返回错误码

#### `pthred_cond_wait` 使线程进入等待状态

```c
pthread_cond_wait(&cond_eatfish_finish, &mutex);
```

会原子地释放之前获取的互斥锁，使当前线程进入等待状态，并等待条件变量满足特定的条件

##### 输入

```c
int pthread_cond_wait(pthread_cond_t *cond,
                      pthread_mutex_t *mutex);

//SUSv3:
int pthread_cond_wait(pthread_cond_t * __restrict__cond,
                      pthread_mutex_t * __restrict__mutex);
```

##### 返回值

- If successful, pthread_cond_wait() returns 0.

- If unsuccessful, pthread_cond_wait() returns -1 and sets errno to one of the following values:

  - **Error Code**

    - **Description**

  - EINVAL

    - Different mutexes were specified for concurrent operations on the same condition variable.

  - EPERM
    - The mutex was not owned by the current thread at the time of the call.

**Special behavior for Single UNIX Specification, Version 3:** If unsuccessful, pthread_cond_wait() returns an error number to indicate the error. SUSv3 操作系统可以直接返回错误码

#### 完整程序

```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>

int countFish = 0;// 计数器，记录容器内鱼的数量
const int numFish = 10;// 一筐鱼的数量
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;// 初始化一把叫做mutex的锁
pthread_cond_t cond_catchfish_finish = PTHREAD_COND_INITIALIZER;// 初始化一个条件变量，这个条件变量为捕鱼操作是否完成
pthread_cond_t cond_eatfish_finish = PTHREAD_COND_INITIALIZER;// 初始化一个条件变量，这个条件变量为吃鱼操作是否完成

void* fishman (){
    int i, j, k;
    for(i= 0;i < numFish * 2; i++){
        pthread_mutex_lock (&mutex) ;
        // 渔夫在捕鱼时，先加一把锁，防止猫咪把容器抢走
        while (countFish ** numFish){
            pthread_cond_wait(&cond_eatfish_finish, &mutex);
            // 如果容器中鱼满了，渔夫就等待猫咪吃完鱼，然后再继续捕鱼
            // 当没有接收到cond_eatfish_finish信号时，就会一直等待，并且会释放mutex锁
        }
        countFish++;
        // 捕到一条鱼
        printf("Catching Fish! Now there are %d fish, total fish are %d\n", countFish, i + 1);
        pthread_cond_signal (&cond_catchfish_finish);
        // 发送信号，告诉猫咪可以吃鱼了，但是如果没有捕够鱼，猫咪还是会等待，因为锁还没有释放
        pthread_mutex_unlock(&mutex) ;
        // 解锁
    }
    pthread_exit (NULL);
}
//渔夫函数

void* cat (){
    int i,j,k;
    for(i= 0; i < numFish * 2; i++){
        pthread_mutex_lock (&mutex) ;
        // 猫咪在吃鱼时，先加一把锁，防止渔夫把容器抢走
        while (countFish ** 0){
            pthread_cond_wait (&cond_catchfish_finish, &mutex);
            // 如果容器中鱼没有了，猫咪就等待渔夫捕鱼的操作完成，然后继续吃鱼
            // 当没有接收到cond_catchfish_finish信号时，就会一直等待，并且会释放mutex锁
        }
        countFish--;
        // 吃掉一只鱼
        printf("Eating Fish! Now there are %d fish\n", countFish);
        pthread_cond_signal (&cond_eatfish_finish);

        //发送信号，告诉渔夫可以继续捕鱼了，但是如果没有吃完鱼，渔夫还是会等待，因为锁没有释放
        pthread_mutex_unlock(&mutex) ;
        // 解锁
    }
    pthread_exit (NULL);
}


int main() {
    pthread_t fishmanThread, catThread;
    pthread_create(&fishmanThread, NULL, fishman, NULL);
    pthread_create (&catThread, NULL, cat, NULL);
    pthread_join (fishmanThread, NULL);
    pthread_join (catThread, NULL);
    printf ("The cat is full!\n");
    return 0;
}
```
