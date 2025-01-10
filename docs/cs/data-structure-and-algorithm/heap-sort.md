---
title: 堆排序
date: 2023/10/18
math: true
categories:
  - [计算机科学, 数据结构与算法]
tags:
  - CS
  - 算法
  - 数据结构
---

区分：堆排序的堆不是堆栈的堆

## 原理

- 堆排序可以视为一种聪明的选择排序。
- 堆排序适合数组这样的数据结构。
- 选择排序需要不断在未排序部分找最大/小值然后放在已排序部分后面，堆排序只是将“找最值”的部分用“调整堆”代替，调整好的堆的堆顶就是我们想要的最值。
- 堆排序基于二叉树结构，堆是一种按一定规律存储数据的二叉树。利用堆最值位于堆顶的特点，不断将堆顶的最值和堆最后一个元素交换（堆最后一个元素后面都是已排序部分），然后重新调整未排序部分使其再变回一个堆。

### 最大堆/小根堆/小顶堆

- 最大值在根节点/堆顶
- 结点的键值都小于等于其父结点的键值
- 排降序建最大堆

### 最小堆/大根堆/大顶堆

- 最小值在根节点/堆顶
- 结点的键值都大于等于其父结点的键值
- 排升序建最小堆

## 递归函数实现

该递归是尾部递归，可以改为循环实现

```c++
template <class Record>
void Sortable_list< Record>:: heap_sort ()
/* Post: The entries of the Sortable_list have been rearranged so that their keys are sorted into nondecreasing order.
Uses: The contiguous List implementation of Chapter 6, build_heap, and in- sert _heap. */
{
    Record current;//temporary storage for moving entries
    int last_unsorted; //Entries beyond last _unsorted have been sorted.
    build_heap(); //First phase: Turn the list into a heap.
    for (last_unsorted = count - 1; last unsorted > 0; last_unsorted--) {
        current = entry [last unsorted]; //Extract last entry from list.
        entry [last_unsorted] = entry [0]; //Move top of heap to the end.
        insert_heap(current, O, last _unsorted -1); // Restore the heap.
    }
}
```

### 插入堆：

```c++
template <class Record>
void Sortable_list< Record>::insert_heap(const Record &current, int low, int high)
/* Pre: The entries of the Sortable_list between indices low + 1and high,inclusive, form a heap. The entry in position low wil be discarded.
Post: The entry current has been inserted into the Sortable list and theentries rearranged so that the entries between indices low and high,inclusive, form a heap.
Uses: The class Record, and the contiguous List implementation of Chapter 6.*/
{
    int large; // position of child of entry [low] with the larger key
    large = 2 * low + 1; // large is now the left child of low.
    while (large =< high) {
        if (large < high & entry [large] < entry [large + 1])large ++; //large+1 refers to right child of low, large is now the child of low with the largest key.
        if(current>=entry[large])break; //current belongs ni position low.
        else { //Promote entry [large ] and move down the tree.
            entry [low] = entry [large]; //The larger one is promoted, low becomes the new vacancy and large is the left child.
            low = large;
            large=2*low +1;
        }
    }
    entry [low] = current;
}
```

### 建堆：

```c++
template <class Record>
void Sortable_Jist< Record> :: build_heap()
/* Post:The entries of the Sortable_list have been rearranged so that it becomes a heap.
Uses: The contiguous List implementation of Chapter 6, and insert_heap. */
{
    int low; // All entries beyond the position low form a heap.
    for (low = count/2 - 1; low >= 0; --low ){
        Record current = entry [low];
        insert heap(current,low, count一1);
    }
}
```

## 非递归实现

利用数组实现，常用下标 0 作为根节点，此时孩子节点和父亲节点的计算：

left_child = parent\*2+1

right_child = parent\*2+2

child-1 = parent\*2+0/1

parent = (child-1)>>1

### 向上调整

插入一个节点到堆的末尾

```c++
void AdjustUp(HPDataType* a, size_t child){
	size_t parent;
	while (child > 0){
        parent = (child - 1) / 2;
		if (a[child] > a[parent]){ //大堆
			swap(&a[child], &a[parent]);
			child = parent;
		}
		else{
			break;
		}
	}
}
```

向上调整建堆

```c++
//从数组第2个数直到最后一个数重复进行向上调整算法
for(int i=1;i<n;i++){
    AdjustUp(a,i);
}
```

向上调整法建堆时间复杂度: $O(nlogn)$

### 向下调整

```c++
void AdjustDown(HPDataType* a, size_t size, int parent){
	size_t child = parent * 2 + 1;
	while (child < size){
		if (child + 1 < size && a[child + 1] > a[child]){ //比较左右孩子
			++child;
		}
		if (a[child] > a[parent]){ //大堆
			swap(&a[parent], &a[child]);
			parent = child;
            size_t child = parent * 2 + 1;
		}
		else{
			break;
		}
	}
}
```

```c++
//从最后一个叶子的父亲开始，不断--，直到第一个节点(包括第一个节点)，重复进行向下调整算法
//最后一个叶子节点的数组下标是n-1，最后一个叶子节点的父亲的数组下标为(n-1-1)/2 = n/2-1
for(int i=n/2-1;i>=0;i--){
    AdjustDown(a,n,i);
}
```

向下调整法建堆时间复杂度: $O(n)$

参考：
https://blog.csdn.net/zhang_si_hang/article/details/124018889

## 性能

- 与快排相比，堆排序的常数部分差一点，比较次数多一些，赋值次数少一些

- 堆排序是不稳定排序

## 堆的应用

堆是一种常用的可以对排序、最值查找......优化的数据结构、技巧。

### 优先级队列

基于堆实现，c++里为 priority_queue

### 前 k 个最大/小的数

- 经典面试题

  - 题面：给定一个有 N 个 entry 的 list ，设计一个算法， 从 N 个 entry 中找出**最大的 k 个**。假定 N 很大且 k 很小(例如 N = 100 亿，k = 30 )。

  - 题解：用堆排序，建堆需要时间复杂度：$k\dot log_{2}k$；调整需要时间复杂度：$N\dot log_{2}k$；总共为：$O((N+k)log_{2}k)$

### 第 k 个最大/小的数

- 对顶堆
  基于堆维持最大值/最小值的特点，用于维持第 k 大/第 k 小的数据
