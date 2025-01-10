---
title: 最短路径问题
date: 2023/12/16
math: true
categories:
  - [计算机科学, 数据结构与算法]
tags:
  - CS
  - 算法
---

## 定义

寻找一个带权**有向**图中某一点到另一个点的最短路径/权最小的路径

当然以下算法也适用于无向图

## Floyd (弗洛伊德) 算法

- 可以算出**所有点之间的最短距离**

```c++
// dist 初始化为邻接矩阵，若i没有指向j的边，则应有 dist[i][j]=MAX_INT(表示无穷大的数)
void floyd(){
    for(int k = 0; k < n; k ++){   //作为循环中间点的k必须放在最外一层循环
        for(int i = 0; i < n; i ++){
            for(int j = 0; j < n; j ++){
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}
//dist[i][j]得出的是i到j的最短路径
```

- 时间复杂度为：$O(n^{3})$

### Warshall 算法

和 Floyd(弗洛伊德)算法很像，二者常常一起统称为 **Warshall-Floyd 算法** ，三重循环一样，只是循环最内层执行的赋值不一样

- 用于由邻接矩阵计算**可达矩阵**

  - 由关系矩阵计算传递闭包矩阵

  - 关系矩阵即为邻接矩阵，对应的传递闭包矩阵即为可达矩阵

```c++
// dist 初始化为邻接矩阵，若存在i指向j的边，则应有 dist[i][j]=1
void floyd(){
    for(int k = 0; k < n; k ++){   //作为循环中间点的k必须放在最外一层循环
        for(int i = 0; i < n; i ++){
            for(int j = 0; j < n; j ++){
                dist[i][j] = dist[i][j] | (dist[i][k] && dist[k][j]);
            }
        }
    }
}
//dist[i][j]得出的可达矩阵
```

或者：

```c++
void floyd(){
    for(int i = 0;i < n;i++){  //先列
		for(int j = 0;j < n;j++){  //后行
			if(matrix[j][i] ** 1){	//	第j行与第i行进行或操作，并最终赋值到第j行
				for(int k = 0;k < n;k++){
					matrix[j][k] = matrix[j][k] | matrix[i][k];
				}
			}
		}
	}
}
```

!!! info rmation
初始化邻接矩阵对角线元素$d[i][i]$为 0，得到的可达矩阵若$d[i][i]=1$则存在包含点 i 的环，从而可以判断图是否有环和找环
- 时间复杂度为：$O(n^{3})$

## Dijkstra (迪杰斯特拉) 算法

可以算出某一个点到其他所有点之间的最短距离，即**单源点最短路径**

对 Prim 算法略加改动就成了 Dijkstra 算法，
将 Prim 算法中求每个顶点$V_k$的 lowcost 值用 dist[k]代替即可。

与 Prim 算法一样用了贪心思想，只有在每条边权值**非负**的情况下才能保证结果正确

- dist[i] 是点 s 出发到 i 的最短路径长度，长度为 n
- 初始需要 dist[i]赋值为 s 出发的边指向的各个点的权
- 剩余不相邻的点全部为 MAX_INT(表示无穷大的数)
- shortest[i] 是标记 s 到点 i 的距离 dist[i]是否确定为最短路径，长度为 n

```c++
void dijkstra(int s){
    memset(shortest, false, sizeof(shortest));
    shortest[s] = true;
    for(int i = 0; i < n; i ++){
        dist[i] = graph[s][i]; //graph[s][i]为s到i的有向边的权
        //初始化dist[i]赋值为s出发的边指向的各个点的权
    }
    int index;
    for(int i = 1; i < n; i ++){ //一共n-1轮后s到其余所有点到距离都是最短距离
        int mincost = MAX_INT; //表示无穷大的数
        for(int j = 0; j < n; j ++){
            if(!shortest[j] && dist[j] < mincost){
                mincost = dist[j];
                index = j;
            }
        } //此处可以改用堆，存储dist中未被确定为最短距离的元素和下标，维护最小值
        shortest[index] = true; //第i轮确定i到index的距离为最短距离
        for(int j = 0; j < n; j ++){
            if(!shortest[j] && dist[j] > dist[index] + graph[index][j]){
                dist[j] = dist[index] + graph[index][j];
            }
        }
    }
}
```

- 时间复杂度为：$O(n^{2})$
