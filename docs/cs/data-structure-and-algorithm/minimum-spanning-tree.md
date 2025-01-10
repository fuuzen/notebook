---
title: 最小生成树
date: 2023/12/22
math: true
categories:
  - [计算机科学, 数据结构与算法]
tags:
  - CS
  - 算法
  - 数据结构
  - 图论
---

## 定义

最小生成树 Minimum Spanning Tree 问题

等价问题：寻找一个带权**无向**图中某一点到另一个点的最短路径/权最小的路径

::: warning
注意:当各边有相同权值时，由于选择的随意性，产生的生成树可能不唯一。
## Kruskal (克鲁斯卡尔) 算法

以**点**为主导，利用**最小堆**和**并查集**

对所有边按权值从小到大排序，逐个添加不形成回路的边，直到包含所有的顶点。

## Prim (普里姆) 算法

- 以**边**为主导
- 贪心思想，不断选最小权值的边

1. 从某一顶点出发，选择与它关联的具有最小权值的边，将其顶点加入到生成树顶点集合 $U_1$ 中，剩余顶点集合为 $U_2$。
2. 以后每一步从：
   - 一个顶点在 $U_1$ 中，
   - 另一个顶点在 $U_2$ 中，
     的所有边中权值最小的边，将 $U_2$ 中该顶点加入到 $U_1$ 中
3. 重复步骤 2 继续 $n-1$ 次直到 $U_2$ 为空

```c++
//lowcost 存放生成树顶点集合内顶点到生成树外各顶点的各边上 的当前最小权值
//nearvex 记录生成树顶点集合外各顶点距离集合内哪个顶点最近(即权值最小)
void Prim(Graph<string>& G, MinSpanTree&T) {
    int i, j, n = G.NumberOfVertices( );//顶点数
    float * lowcost = new float[n];
    int * nearvex = new int[n];
    for(i=1;i<n;i++){
        lowcost[i] = G.GetWeight(0, i); //顶点0到各边代价
        nearvex[i] = 0; //及最短带权路径
    }
    nearvex[0] = -1;//加到生成树顶点集合
    MSTEdgeNode e;//最小生成树结点单元
    for ( i = 1; i < n; i++ ) {//循环n-1次, 加入n-1条边
        float min = MaxValue;
        int v = 0;
        for ( j = 0; j < n; j++ ){
            if ( nearvex[j] != -1 && lowcost[j] < min ) {
                v=j;
                min=lowcost[j];
            }
        }
        //求生成树外顶点到生成树内顶点具有最小权值的边, v是当前具最小权值的边
        if ( v ) { //v=0表示再也找不到要求顶点
            e.tail = nearvex[v];
            e.head = v;
            e.cost = lowcost[v];
            T.Insert (e); //选出的边加入生成树 //若是计算MST总权值则是加该边的权值
            nearvex[v]=-1; //该边加入生成树标记
            for ( j = 1; j < n; j++ ){
                if ( nearvex[j] != -1 && G.GetWeight(v, j) < lowcost[j] ) {
                    lowcost[j] = G.GetWeight(v, j);
                    nearvex[j] = v;
                }
            }
        }
    }
    free loecodt; free nearex;
}
```

### 两种算法比较

分析普里姆算法, 设连通网络有 n 个顶点，则该算法的时间复杂度为$O(n^2)$，它适用于边稠密的网络，与边的数目无关。
克鲁斯卡尔算法主要针对边展开，边数少时效率会很高，所以对于边稀疏的图有优势。
