---
title: 关键路径问题
date: 2023/12/01
math: true
categories:
  - [计算机科学, 数据结构与算法]
tags:
  - CS
  - 算法
  - 数据结构
  - 图论
---

## AOV: Activity On Vertex

略（不考）

## AOE: Activity On Edge

与 AOV 网相对应的是 AOE (Activity On Edge) ，是边表示活动的有向无环图，如图所示。图中顶点表示事件(Event)，每个事件表示在其前的所有活动已经完成，其后的活动可以开始;弧表示活动，弧上的权值表示相应活动所需的时间或费用

与 AOE 有关的研究问题

- 完成整个工程至少需要多少时间?
- 哪些活动是影响工程进度(费用)的关键?

工程完成**最短时间**:从起点到终点的**最长路径长度**(路径上各活动持续时间之和)

长度最长的路径称为**关键路径**，关键路径上的活动称为**关键活动**。 关键活动是影响整个工程的关键。

### 求 AOE 中关键路径和关键活动

设$v_0$是起点，从$v_0$到$v_i$的最长路径长度称为事件$v_i$的最早发生时间，即是 以$v_i$为尾的所有活动的最早发生时间。

若活动$a_i$是弧$<j,k>$，持续时间是$dut(<j,k>)$,设：

$e(i)$:表示活动$a_i$的最早开始时间;

$l(i)$:在不影响进度的前提下，表示活动$a_i$的最晚开始时间;

$l(i)-e(i)$表示活动$a_i$的时间余量，若$l(i)-e(i)=0$，表示活动$a_i$是关键活动

$v_e(i)$:表示事件$v_i$的最早发生时间，即从起点到顶点$v_i$的最长路径长度

$v_l(i)$:表示事件$v_i$的最晚发生时间

则有以下关系:

$$
\begin{aligned}
e(i) &= v_e(j)\\
l(i) &= v_l(k)-dut(<j,k>)
\end{aligned}
$$

并且：

$$
\begin{aligned}
v_e(j) = \left\{
    \begin{aligned}
    &0\\
    &Max\{ve(i)+dut(<i, j>)|<v_i, v_j>是网中的弧\}
    \end{aligned}
    \begin{aligned}
    j=0,表示vj是起点\\
    j\neq 0
    \end{aligned}
\right.
\end{aligned}
$$

含义是:源点事件的最早发生时间设为 0;除源点外，只有进入顶点$v_j$的所有弧所代表的活 动全部结束后，事件$v_j$才能发生。即只有$v_j$的所有前驱事件$v_i$的最早发生时间$v_e(i)$计算出来 后，才能计算$v_e(j)$。

方法是:对所有事件进行拓扑排序，然后依次按拓扑顺序计算每个事件的最早发生时间。

$$
\begin{aligned}
v_l(j) =  \left\{
    \begin{aligned}
    &v_e(n-1) \\
    &Min\{v_l(k)-dut(<j, k>)|<v_j, v_k>是网中的弧\}
    \end{aligned}
    \begin{aligned}
    j=n-1，表示v_j是终点\\
    j\neq n-1
    \end{aligned}
\right.
\end{aligned}
$$

含义是:只有$v_j$的所有后继事件$v_k$的最晚发生时间$v_l(k)$计算出来后，才能计算$v_l(j)$ 。

方法是:按拓扑排序的逆顺序，依次计算每个事件的最晚发生时间。

#### 算法思想

1. 利用拓扑排序求出 AOE 网的一个拓扑序列;
2. 从拓扑排序的序列的第一个顶点(源点)开始，按**拓扑顺序**依次计算每个事件的**最早发生时间**$v_e(i)$;

3. 从拓扑排序的序列的最后一个顶点(汇点)开始，按**逆拓扑顺序**依次计算每个事件的**最晚发生时间**$v_l(i)$;

#### 代码实现

```c++
void critical_path(ALGraph *G) //Adjacency List Graph
{
    int j, k, m ; LinkNode *p ;
    if (Topologic_Sort(G)**-1)
        printf("\nAOE网中存在回路,错误!!\n\n");
    else
    {
        for (j=0; j<G->vexnum; j++)
            ve[j]=0; // 事件最早发生时间初始化
        for (m=0 ; m<G->vexnum; m++)
        {
            j=topol[m] ;
            p=G->adjlist[i].firstarc ;
            for(; pl=NULL; p=p->nextarc )
            {
                k=p->adjvex;
                if (ve[j]+p->weight>ve[k])
                    ve[k]=ve[j]+p->weight ;
            }
        }//计算每个事件的最早发生时间ve值
        for ( j=0; j<G->vexnum; j++)
            vl[j]=ve[j];//事件最晚发生时间初始化
        for (m=G->vexnum-1; m>=0, m--)
        {
            j=topol[m] ;
            p=G->adjlist[i].firstarc;
            for (; pl=NULL; p=p->nextarc)
            {
                k=p->adjvex ;
                if (vl[k]-p->weight<vl[j])
                    vl[i]=$v_i$[k]-p->weight ;
            }
        }//计算每个事件的最晚发生时间vl值
        for (m=0 , m<G->vexnum; m++)
        {
            p=G->adjlist[m].firstarc ;
            for(; p!=NULL; p=p->nextarc )
            {
                k=p->adjvex;
                if ( (ve[m]+p->weight)**l[k])
                    printf("<%d, %d>, m, j");
            }
        }//输出所有的关键活动
    }//end of else
}
```

#### 算法分析

设 AOE 网有 n 个事件，e 个活动，则算法的主要执行是:

- 进行拓扑排序:时间复杂度是$O(n+e)$

- 求每个事件的 ve 值和 vl 值:时间复杂度是$O(n+e)$

- 根据 ve 值和 vl 值找关键活动:时间复杂度是$O(n+e)$

因此，整个算法的时间复杂度是$O(n+e)$
