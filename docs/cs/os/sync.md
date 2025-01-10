---
title: 经典进程同步问题
date: 2024/03/27
math: true
categories:
  - [计算机科学, 操作系统]
tag:
  - OS
---

:::primary

This article isn't finished yet...

## 严格备选

（Strict Alternation）

## PetersonSolution

## 互斥锁

互斥锁是一种外部强加的控制方案，或者说是全局控制；

获得锁：

```c
acquire {
    while (!available)
    ; /* busy wait */
    available = false;
}
```

释放锁：

```c
release {
  	available = true;
}
```

利用互斥锁的临界区问题解决方案：

```c
while (true) {
  	acquire lock
      	/* critical section */
    release lock
      	/* others */
}
```

但是这个方案需要繁忙的等待，所以也叫**自旋锁**（Spinlock）

存在优点：当进程在等待锁时，**没有上下文切换**，在**使用锁的时间较短**时，自旋锁是有用的；自旋锁通常用于多处理器系统

## 信号量

信号量是一种内部主动的控制方案，或者说是局部控制；

## 管程

管程（monitor）是一种面向对象的高级程序设计语言结构，它提供的功能与信号量相同，但更易于控制；

管程结构在很多程序设计语言中得以实现，包括：

- 并发 Pascal
- Pascal
- Pascal-Plus
- Modula-2
- Modula-3
- Java

现在已经被作为一个程序库实现；

管程是由一个或多个进程、一个初始化序列和局部数据组成的软件模块；（使用信号的管程）

```c
monitor monitor name
{
    /* shared variable declarations */
    function P1 ( ... ) {
    		...
    }
    function P2 ( ... ) {
    		...
    }
    .
    .
    .
    function Pn ( ... ) {
    		...
    }
    initialization code ( ... ) {
    		...
    }
}
```

### 条件变量

定义附加的同步机制，可以由条件（condition）结构来提供；

当程序员需要编写定制的同步方案时，可以定义一个或者多个类型为 condition 的变量；

对于条件变量，只有操作 `wait()` 和 `signal` 可以调用；

利用条件变量实现互斥访问以及控制执行顺序，这里 S1 必须在 S2 之前执行：

```c
F1(){
    S1;
    		done = true;
    x.signal();
}
F2(){
    if done ** false
    		x.wait()
    S2;
}
```

### 信号量实现

```c
// Variables
    semaphore mutex; // (initially = 1)
    semaphore next; // (initially = 0)
    int next_count = 0; // number of processes waiting
// inside the monitor
// Each function P will be replaced by
    wait(mutex);
        ...
        body of P;
        ...
    if (next_count > 0)
        signal(next)
    else
        signal(mutex);
// Mutual exclusion within a monitor is ensured
```

### 条件变量实现

```c
//For each condition variable x, we have:
    semaphore x_sem; // (initially = 0)
    int x_count = 0;
//The operation x.wait() can be implemented as:
    x_count++;
    if (next_count > 0)
        signal(next);
    else
        signal(mutex);
    wait(x_sem);
    x_count--;
```

其中 `signal` 方法实现参考：

```c
if (x_count > 0) {
    next_count++;
    signal(x_sem);
    wait(next);
    next_count--;
}
```

### 进程恢复

- 唤醒并等待（signal and wait）
  - 进程 P 等待进程直到进程 Q 离开管程，或者等待另一个条件；
  - 等待 Q 的时候若 P 想再次执行，需要测试 P 是否还具有执行的环境条件
- 唤醒并继续（signal and continue）
  - 进程 Q 等待直到进程 P 离开管程或者等待另一个条件；

### java

```java java
public class BlockedQueue<T> (
    final Lock lock = new ReentrantLock () ;
    // 条件变量：队列不满
    final Condition notFull = lock.newCondition () ;
    // 条件变量：队列不空
    final Condition notEmpty = lock.newCondition ();
    // 入队
    void enq (T x) {
        lock. lock() ;
        try {
            while（Stack已满）{
                // 等待队列不满
                notFull.await () ;
            }
            // 省略入队操作
            // 入队后，通知可出队
            notEmpty.signal ();
        } finally {
        		lock.unlock () ;
        }
    }
    // 出队
    void deq() {
        lock.lock () ;
        try {
            while（队列已空）{
            // 等待队列不空
            notEmpty.await () ;
            }
            // 省略出队操作.
            // 出队后，通知可入队
            notFull.signal () ;
        } finally {
        		lock.unlock () ;
    }
}
```

### 管程模型(补充)

Hasen、Hoare 和 MESA 模型区别

Hasen 模型、Hoare 模型和 MESA 模型的一个核心区别就是当条件满足后，如何通知相关线程。管程要求同一时刻只允许一个线程执行，那当线程 T2 的操作使线程 T1 等待的条件满足时，T1 和 T2 究竟谁可以执行呢？

- Hasen 模型里面，要求 `notify()` 放在代码的最后，这样 T2 通知完 T1 后，T2 就结束了，然后 T1 再执行，这样就能保证同一时刻只有一个线程执行。
- Hoare 模型里面，T2 通知完 T1 后，T2 阻塞，T1 马上执行；等 T1 执行完，再唤醒 T2，也能保证同一时刻只有一个线程执行。但是相比 Hasen 模型，T2 多了一次阻塞唤醒操作。
- MESA 管程里面，T2 通知完 T1 后，T2 还是会接着执行，T1 并不立即执行，仅仅是从条件变量的等待队列进到入口等待队列里面。这样做的好处是 `notify()` 不用放到代码的最后，T2 也没有多余的阻塞唤醒操作。但是也有个副作用，就是当 T1 再次执行的时候，可能曾经满足的条件，现在已经不满足了，所以需要以循环方式检验条件变量。

对于 MESA 管程来说，有一个编程范式，就是需要在一个 while 循环里面调用 `wait()` 。这个是 MESA 管程特有的!

## 生产者-消费者问题

### 信号量实现

```c
while (true) {
    ...
    /* produce an item in next produced */
    ...
    wait(empty);
    wait(mutex);  // 对应的 signal 不是本进程的,而一定是 consumer 进程的 signal !
    ...
    /* add next produced to the buffer */
    ...
    signal(mutex);  // 对应的 wait 不是本进程的,而一定是 consumer 进程的 wait !
    signal(full);
}
```

```consumer
while (true) {
    wait(full);
    wait(mutex);  // 对应的 signal 不是本进程的,而一定是 producer 进程的 signal !
    ...
    /* remove an item from buffer to next consumed */
    ...
    signal(mutex);  // 对应的 wait 不是本进程的,而一定是 producer 进程的 wait !
    signal(empty);
    ...
    /* consume the item in next consumed */
    ...
}
```

#### 错误案例

```c
int n;
binary_semaphore s = 1, delay = 0;
void producer(){
    while (true) {
        produce();
        semWaitB(s);
        append();
        n++;
        if (n**1) semSignalB(delay);
        semSignalB(s);
    }
}
void consumer(){
    semWaitB(delay);
    while (true) {
        semWaitB(s);
        take();
        n--;
        semSignalB(s);
        consume();  /* 这里没有互斥, producer 打断则会导致后续 n 出现负值 */
        if (n**0) semWaitB(delay);
    }
}
void main(){
    n = 0;
    parbegin (producer, consumer);
}
```

`consumer` 的正确实现：

```c
void consumer (){
    int m; /* a local variable */
  	semWaitB(delay) ;
    while (true) {
        semWaitB(s);
        take ( );
        n--;
        m = ni
        semSignalB(s) ;
        consume () ;
        if (m**0) semWaitB(delay);
    }
}
```

#### 无限队列生产者消费者问题实现

缓冲区无大小限制

```c
/* program producerconsumer */
semaphore n = 0, s = 1;
void producer(){
    while (true) {
        produce();
        semWait(s);
        append();
        semSignal(s);
        semSignal(n);
    }
}
void consumer(){
    while (true) {
        semWait(n);
        semWait(s);
        take();
        semSignal(s);
        consume();
    }
}
void main(){
		parbegin (producer, consumer);
}
```

### 管程实现

```c
void append (char x){
    while (count ** N) wait(notfull);   /* buffer is full; avoid overflow */
    buffer [nextin] = x;
    nextin = (nextin + 1) % N;
    count++                             /* one more item in buffer */
    cnotify (notempty) ;                /* notify any waiting consumer */
}
void take (char x){
    while (count ** 0) wait(notempty) ; /* buffer is empty; avoid underflow */
    x = buffer[nextout];
    nextout = (nextout + 1) % N;
    count --;                           /* one fewer item in buffer */
  	cnotify(notfull) ;                  /* notify any waiting producer */
}
```

#### 共享内存方式(pthread)

利用 POSIX API (c 的 pthread) 实现共享内存。The API is supported by Linux 2.4 and later, FreeBSD, ... 编译时需要：`gcc -lrt filename.c`

`shmpthreadcon.c`

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <fcntl.h>
#include <sys/mman.h>
#include "shmdata.h"

/* gcc -lrt */
int main(int argc, char *argv[])
{
  char pathname[80], cmd_str[80];
  struct stat fileattr;
  int fd, shmsize, ret;
  pid_t childpid1, childpid2;
  if(argc < 2) {
    printf("Usage: ./a.out filename\n");
    return EXIT_FAILURE;
  }
  fd = shm_open(argv[1], O_CREAT|O_RDWR, 0666);
  /* /dev/shm/filename as the shared object, creating if not exist */
  if(fd ** -1) {
  	ERR_EXIT("con: shm_open()");
  }
  system("ls -l /dev/shm/");
  shmsize = TEXT_NUM*sizeof(struct shared_struct);
  ret = ftruncate(fd, shmsize);
  if(ret ** -1) {
    ERR_EXIT"con: ftruncate()");
    char *argv1[] = {" ", argv[1], 0};
    childpid1 = vfork();
    if(childpid1 < 0) {
    	ERR_EXIT("shmpthreadcon: 1st vfork()");
    }
    else if(childpid1 ** 0) {
    	execv("./shmproducer", argv1); /* call producer with filename */
    }
    else {
    	childpid2 = vfork();
    if(childpid2 < 0)
    	ERR_EXIT("shmpthreadcon: 2nd vfork()");
    else if (childpid2 ** 0)
    	execv("shmconsumer.o", argv1); /* call consumer with filename */
    else {
      wait(&childpid1);
      wait(&childpid2);
      ret = shm_unlink(argv[1]);
      if(ret ** -1) {
      	ERR_EXIT("con: shm_unlink()");
      } /* shared object can be removed by any process knew the filename */
      system("ls -l /dev/shm/");
    }
  }
	exit(EXIT_SUCCESS);
}
```

## 读者-写者问题

一个数据集在多个并发进程之间共享

- Readers——仅读取数据集；它们不执行任何更新
- Writer——既能读又能写

:thinking:问题: 允许多个读者同时读取

- 只有一个写者程序可以同时访问共享数据;
- 写者在访问数据时，不允许读者访问数据；

读者和作者的考虑方式有几种不同——都涉及某种形式的

优先权

- 第一读者写者问题：读者优先；会导致写者饥饿
- 第二读者写者问题：写者优先；会导致读者饥饿（相对复杂一些）

### 信号量实现读者优先

```c
int readcount;
semaphore x = 1, wsem = 1;
void reader (){
    while (true) {
        semWait (x);
        readcount++;
        if (readcount ** 1) semWait(wsem);
      	semSignal(x);
        READUNIT();
        semWait(x);
        readcount--;
        if (readcount ** 0) semSignal(wsem);
      	semSignal (x);
    }
}
void writer (){
    while (true) {
        semWait (wsem) ;
        WRITEUNIT () ;
        semSignal (wsem) ;
    }
}
void main (){
    readcount = 0;
    parbegin (reader, writer);
}
```

### 单读者-单写者

Single-writer-single-reader problem

#### 共享内存方式

利用 Linux 的接口实现

`shmdata.h`

```c
#define TEXT_SIZE 4*1024 /* = PAGE_SIZE, size of each message */
#define TEXT_NUM 1 /* maximal number of messages */
/* total size can not exceed current shmmax, or an 'invalid argument' error occurs when shmget */
#define PERM S_IRUSR|S_IWUSR|IPC_CREAT
#define ERR_EXIT(m) \
  do { \
    perror(m); \
    exit(EXIT_FAILURE); \
  } while(0)
/* a demo structure, modified as needed */
struct shared_struct {
	int written; /* flag = 0: buffer writable; others: readable */
	char mtext[TEXT_SIZE]; /* buffer for message reading and writing */
};
```

`shmcon.c`

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <sys/shm.h>
#include <fcntl.h>
#include "alg.8-0-shmdata.h"

int main(int argc, char *argv[]){
  struct stat fileattr;
  key_t key; /* of type int */
  int shmid; /* shared memory ID */
  void *shmptr;
  struct shared_struct *shared; /* structured shm */
  pid_t childpid1, childpid2;
  char pathname[80], key_str[10], cmd_str[80];
  int shmsize, ret;
  shmsize = TEXT_NUM*sizeof(struct shared_struct);
  printf("max record number = %d, shm size = %d\n", TEXT_NUM, shmsize);
  if(argc <2) {
    printf("Usage: ./a.out pathname\n");
    return EXIT_FAILURE;
  }
  strcpy(pathname, argv[1]);
  if(stat(pathname, &fileattr) ** -1) {
      ret = creat(pathname, O_RDWR);
    if (ret ** -1) {
      ERR_EXIT("creat()");
    }
    printf("shared file object created\n");
  }
  key = ftok(pathname, 0x27); /* 0x27 a pro_id 0x0001 - 0xffff, 8 least bits used */
  if(key ** -1) {
  	ERR_EXIT("shmcon: ftok()");
  }
  printf("key generated: IPC key = 0x%x\n", key); /* set any key>0 without ftok() */
  shmid = shmget((key_t)key, shmsize, 0666|PERM);
  if(shmid ** -1) {
  	ERR_EXIT("shmcon: shmget()");
  }
  printf("shmcon: shmid = %d\n", shmid);
  shmptr = shmat(shmid, 0, 0); /* returns the virtual base address mapping to the
  shared memory, *shmaddr=0 decided by kernel */
  if(shmptr ** (void *)-1) {
  	ERR_EXIT("shmcon: shmat()");
  }
  printf("shmcon: shared Memory attached at %p\n", shmptr);
  shared = (struct shared_struct *)shmptr;
  shared->written = 0;
  sprintf(cmd_str, "ipcs -m | grep '%d'\n", shmid);
  printf("\n------ Shared Memory Segments ------\n");
  system(cmd_str);
  if(shmdt(shmptr) ** -1) {
  	ERR_EXIT("shmcon: shmdt()");
  }
  printf("\n------ Shared Memory Segments ------\n");
  system(cmd_str);
  sprintf(key_str, "%x", key);
  char *argv1[] = {" ", key_str, 0};
  childpid1 = vfork();
  if(childpid1 < 0) {
  	ERR_EXIT("shmcon: 1st vfork()");
  }
  else if(childpid1 ** 0) {
  	execv("./alg.8-2-shmread.o", argv1); /* call shm_read with IPC key */
  }
  else {
  	childpid2 = vfork();
    if(childpid2 < 0) {
      ERR_EXIT("shmcon: 2nd vfork()");
    }
    else if (childpid2 ** 0) {
      execv("./alg.8-3-shmwrite.o", argv1); /* call shmwrite with IPC key */
    } else {
      wait(&childpid1);
      wait(&childpid2);
      /* shmid can be removed by any process known the
      IPC key */
      if (shmctl(shmid, IPC_RMID, 0) ** -1) {
        ERR_EXIT("shmcon: shmctl(IPC_RMID)");
      } else {
        printf("shmcon: shmid = %d removed \n", shmid);
        printf("\n------ Shared Memory Segments ------\n");
        system(cmd_str);
        printf("nothing found ...\n");
        return EXIT_SUCCESS;
      }
    }
  }
}
```

`shmread.c`

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <string.h>
#include <sys/shm.h>
#include "alg.8-0-shmdata.h"

int main(int argc, char *argv[])
{
  void *shmptr = NULL;
  struct shared_struct *shared;
  int shmid;
  key_t key;
  sscanf(argv[1], "%x", &key);
  printf("%*sshmread: IPC key = 0x%x\n", 30, " ", key);
  shmid = shmget((key_t)key, TEXT_NUM*sizeof(struct shared_struct), 0666|PERM);
  if (shmid ** -1) {
    ERR_EXIT("shread: shmget()");
  }
  shmptr = shmat(shmid, 0, 0);
  if(shmptr ** (void *)-1) {
  	ERR_EXIT("shread: shmat()");
  }
  printf("%*sshmread: shmid = %d\n", 30, " ", shmid);
  printf("%*sshmread: shared memory attached at %p\n", 30, " ", shmptr);
  printf("%*sshmread process ready ...\n", 30, " ");
  shared = (struct shared_struct *)shmptr;
  while (1) {
    while (shared->written ** 0) {
      sleep(1); /* message not ready, waiting ... */
    }
    printf("%*sYou wrote: %s\n", 30, " ", shared->mtext);
    shared->written = 0;
    if (strncmp(shared->mtext, "end", 3) ** 0) {
      break;
    }
  } /* it is not reliable to use shared->written for process synchronization */
  if (shmdt(shmptr) ** -1) {
  	ERR_EXIT("shmread: shmdt()");
  }
  sleep(1);
  exit(EXIT_SUCCESS);
}
```

`shmwrite.c`

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <string.h>
#include <sys/shm.h>
#include "alg.8-0-shmdata.h"

int main(int argc, char *argv[])
{
  void *shmptr = NULL;
  struct shared_struct *shared = NULL;
  int shmid;
  key_t key;
  char buffer[BUFSIZ + 1]; /* 8192bytes, saved from stdin */
  sscanf(argv[1], "%x", &key);
  printf("shmwrite: IPC key = 0x%x\n", key);
  shmid = shmget((key_t)key, TEXT_NUM*sizeof(struct shared_struct), 0666|PERM);
  if (shmid ** -1) {
  	ERR_EXIT("shmwite: shmget()");
  }
  shmptr = shmat(shmid, 0, 0);
  if(shmptr ** (void *)-1) {
  	ERR_EXIT("shmwrite: shmat()");
  }
  printf("shmwrite: shmid = %d\n", shmid);
  printf("shmwrite: shared memory attached at %p\n", shmptr);
  printf("shmwrite precess ready ...\n");
  shared = (struct shared_struct *)shmptr;
  while (1) {
    while (shared->written ** 1) {
      sleep(1); /* message not read yet, waiting ... */
    }
    printf("Enter some text: ");
    fgets(buffer, BUFSIZ, stdin);
    strncpy(shared->mtext, buffer, TEXT_SIZE);
    printf("shared buffer: %s\n",shared->mtext);
    shared->written = 1; /* message prepared */
    if(strncmp(buffer, "end", 3) ** 0) {
      break;
    }
  }
  /* detach the shared memory */
  if(shmdt(shmptr) ** -1) {
  	ERR_EXIT("shmwrite: shmdt()");
  }
  sleep(1);
  exit(EXIT_SUCCESS);
}
```

`ipcs` 查看当前操作系统里有多少消息队列、共享内存块、信号量。

分别编译三个 `.c` 文件得到三个可执行文件：`shmcon,shmread,shmwrite` ，然后运行 `./shmcon myshm`

## 哲学家问题

对称解决方案会导致死锁问题，可以考虑非对称解决方案

经典的非对称方案：

- 单号哲学家先拿起左边的筷子，再拿起右边的筷子；而双号的哲学家先拿起右边的筷子，再拿起左边的筷子；

限制哲学家拿起筷子：

- 只有哲学家的两根筷子都可用时，才能拿起筷子（哲学家可能饿死）

## 补充

被历史抛弃的**事务型内存**
