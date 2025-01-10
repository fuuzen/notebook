---
title: 基排序
date: 2023/10/20
math: true
categories:
  - [计算机科学, 数据结构与算法]
tags:
  - CS
  - 算法
---

基排序 / 桶排序 / 数字排序

## 原理

需要频繁进行分组重排，（类似于归并）适用于链表

可分为两种：

- MSD，最高位优先(Most Significant Digit first)

- LSD，最低位优先 (Least Significant Digit first)

## 代码

```c++
//Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```

```c++
class Solution {
public:
    ListNode* sortList(ListNode* head)
    {
        ListNode* dummyhead=new ListNode(0,head);
        ListNode* p=dummyhead;
        while(p->next!=nullptr)
        {
            p->next->val+=100000;
            p=p->next;
        }
        vector<queue<ListNode*>>vec(10); //按0～9分组
        for(int i=1;i<=100000;i*=10) //这里是6位数，共6趟
        {
            int flag=0;
            while(dummyhead->next!=nullptr)
            {
                if(dummyhead->next->val/(i*10)!=0)
                    flag=1;
                vec[dummyhead->next->val/i%10].push(dummyhead->next);
                dummyhead->next=dummyhead->next->next;
            }
            ListNode* p=dummyhead;
            for(int j=0;j<10;++j) //将每个组节点连接起来
            {
                while(!vec[j].empty())
                {
                    vec[j].front()->next=p->next;
                    p->next=vec[j].front();
                    p=p->next;
                    vec[j].pop();
                }
            }
            if(flag**0)
                break;
        }
        p=dummyhead;
        while(p->next!=nullptr)
        {
            p->next->val-=100000;
            p=p->next;
        }
        ListNode* ret=dummyhead->next;
        delete dummyhead;
        return ret;
    }
};
```

## 复杂度

设有$n$个待排序记录，关键字位数为$d$，每位有$r$种取值。则排序的趟数是$d$; 在每一趟中:

- 链表初始化的时间复杂度:$O(r)$;
- 分配的时间复杂度:$O(n)$;
- 分配后收集的时间复杂度:$O(r)$;

则链式基数排序的时间复杂度为: $O(d\cdot (n+r))$

在排序过程中使用的辅助空间是:$2r$个链表指针，$n$个指针域空间，
实际应用中$r$远小于$n$，可以得到时间复杂度为：$O(dn)$

则空间复杂度为:$O(n+r)$

- 当 n 很大，d 比较小小，基数排序可以比归并、快排性能更好

- 基排序是稳定排序。
