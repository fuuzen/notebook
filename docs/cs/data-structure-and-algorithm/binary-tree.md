---
title: 二叉树
date: 2023/11/03
math: true
categories:
  - [计算机科学, 数据结构与算法]
tags:
  - CS
  - 算法
  - 图论
---

## 二叉树的一些规范(考研)

节点的度数：节点出发指向下一层节点的边的数量(出度)

$N_0,N_1,N_2$：分别表示度数为 1，2，3 的顶点数量

### 结论

任意二叉树满足：

- $n=N_0+N_1+N_2$

- $n-1=0\times N_0+1\times N_1+2\times N_2=N_1+2N_2$

- $n=1+N_1+2N_2$

- $N_2=N_0-1$

正则二叉树还满足：

- $N_1=0$

- $n+1=2N_0$

- $n-1=2N_2$

## 树的遍历

- 先序：根、左、右

- 中序：左、根、右

- 后序：左、右、根

### 递归做法

三行代码：遍历左子树、节点、右子树，变换顺序即可得到三种遍历方式

### 非递归做法

略

## 已知二叉树的两种遍历结果确定(还原)二叉树

- 即还原这颗二叉树，当且仅当这两种遍历中有一个是中序遍历！

- 如果遍历结果是打印出来的元素结果，还需要每个元素不一样？！结点序列的话由于是指针则不需要？

- 基本思路：已知先序和后序遍历都能容易确定谁是树根，再由中序遍历才确定左右子树

## 线索二叉树 Threaded binary trees

一个具有 n 个结点的二叉树若采用二叉链表存储结构， 在 2n 个指针域中只有 n-1 个指针域是用来存储结点孩 子的地址，而另外 n+1 个指针域存放的都是 NULL。

因此，可以利用某结点空的左指针域 (lchild) 指出该 结点在某种遍历序列中的直接前驱结点的存储地址， 利用结点空的右指针域 (rchild) 指出该结点在某种遍历序列中的直接后继结点的存储地址;对于那些非空 的指针域，则仍然存放指向该结点左、右孩子的指针。 这样，就得到了一棵线索二叉树。

由于序列可由不同的遍历方法得到，因此，线索树有 先序线索二叉树、中序线索二叉树、后序线索二叉树 三种。把二叉树改造成线索二叉树的过程称为线索化。

## 哈夫曼树

哈夫曼树(Huffman Tree)是指对于一组带有确定权值的叶结点，构造的具有最小带权路径长度的二叉树，也称**最优二叉树**(Optimal Binary Tree)。

设二叉树具有 n 个带权值的叶结点，那么从根结点到各个叶结点的路径长度与 相应结点权值的乘积之和，称为二叉树的带权路径长度，记为:

$WPL=\sum_{k=1}^{n}{W_k\times L_k}$

其中 $W_k$ 为第 k 个叶结点的权值，$L_k$ 为第 k 个叶结点的路径长度。

在给定一组具有确定权值的叶结点，可以构造出不同的带权二叉树。这些形状不同的二叉树的带权路径长度 WPL 将各不相同。

哈夫曼树是**正则二叉树**，即没有度数为 1 的顶点。

### 构造

由相同权值的一组叶结点所构成的二叉树有不同的形态和不同的带权路径长度，那么如何找到带权路径长度最小的二叉树呢?

根据哈夫曼树的定义，一棵二叉树要使其 WPL 值最小，必须使权值越大的叶结点越靠近根结点，而权值越小的叶结点越远离根结点。

哈夫曼(Huffman)依据这一特点提出了一种方法。

1. 由给定的 n 个权值{$W_1$,$W_2$,...,$W_n$}构造 n 棵只有一个叶结点的二叉树，从而得到 一个二叉树的集合 F={$T_1$,$T_2$,...,$T_n$};

2. 在 F 中选取根结点的权值最小和次小的两棵二叉树作为左、右子树构造一棵新的二叉树，这棵新的二叉树根结点的权值为其左、右子树根结点权值之和;

3. 在集合 F 中删除作为左、右子树的两棵二叉树，并将新建立的二叉树加入到集合 F 中;

4. 重复(2)、(3)两步，当 F 中只剩下一棵二叉树时，这棵二叉树便是所要建立的哈夫曼树。

### 哈夫曼编码

哈夫曼编码(Huffman Coding)是一种编码方式，是一种用于无损数据压 缩的熵编码(权编码)算法，是 David A. Huffman 发明的。

#### 哈夫曼编码代码

```c++
typedef struct  //哈夫曼编码的存储结构
{ char bits[N];  //保存编码的数组
    int start;   //编码有效起始位置，该位置之后的01串为字符的编码
    char ch;  //字符
} CodeType ;
CodeType code[N+1]; //字符编码数组，下标为0的单元空出
```

```c++
huffmanCode(HufmTree tree[],CodeType code[]) /*利用哈夫曼树求字符的哈夫曼编码*/
{
    int i,c,p;
    for(i = 1; i<=N; i++)  // N次循环，分别得到N个字符的编码
    {
        code[i].start = N; c = i;
        p = tree[i].parent; // 获取字符的双亲
        while (p != 0) // 一直往上层查找，直到根结束
        {
            code[i].start--;
            if ( tree[p].lchild **c ) code[i].bits[code[i].start] = '0';
            else code[i].bits[code[i].start] = '1';
            c = p;
            p = tree[p].parent;
        }
    }
}
```

## 二叉查找树

BST(Binary Search Tree)递归定义：

1. 根节点：左子树所有节点的值 < 根节点的值 < 右子树所有节点的值

2. 任何节点的左子树右子树也都是 BST

### 二叉树的查找(二叉查找树实现二分查找函数)

递归实现：

```c++
//Recursive auxiliary function
template <class Record>
Binary_node<Record>*
Search_tree<Record> :: search_for_node
(Binary_node<Record>*root1, const Record &target)const
{
    if (root1 ** NULL || root1->data ** target) return root;
    if (root1->data < target)return search_for_node(root1->right, target);
    return search_for_node(root1->left, target);
}
```

```c++
//Nonrecursive version
template <elass Record>
Binary_node<Record>*
Search_tree<Record> :: search_for_node
(Binary_node<Record>*root1, const Record &target) const
{
    while (root1 != NULL && root1->data != target){
        if (root1->data < target) root1 = root1->right;
        else root1 = root1->left;
    }
    return root1:
}
```

```c++
//Public method for tree search
template <class Record>
Error_code
Search_tree<Record> :: tree_search(Record &target) const
{
    Error_code_result = success;
    Binary_node<Record> *found = search_for_node(root, target);
    if (found ** NULL) result = not_present;
    else target = found->data;
    return result;
}
```

### BST 的插入

```c++
template <class Record>
Error_code
Search_tree<Record> :: insert(const Record & new_data)
{
    return search_and_insert(root, new_data);
}
```

```c++
template <class Record>
Error_code
Search_tree<Record> :: search_and_insert
(Binary_node<Record> *&root1, const Record & new_data)
{
    if (root1 ** NULL)
    {
        root1 = new Binary_node<Record>(new_data);
        return success;
    }
    if (root1->data ** new_data) return duplicate_error;
    if (root1->data > new_data) return search_and_insert(root1->left, new_data);
    return search_and_insert root1->right, new_data);
}
```

### BST 的遍历

BST 中序遍历的结果就是所有数据升序排序结果

### BST 的排序

实际上与快排等价

平均情况下 BST 排序(Treesort)的时间复杂度为：

$2nln(n)+O(n)\approx1.39nlgn+O(n)$

### BST 的删除

1. 要删除的节点没有子节点
   直接删除即可

2. 要删除的节点只有一个子节点（子树）
   以子树根节点替换该节点再删除该节点即可

3. 要删除的节点有两个子节点（子树）
   要删除节点 x
   有两种互相对称的做法：
   1. 沿着左子树的右链走到尽头的节点 w 的元素是 x 左子树里最大的元素
      只需将 w 元素赋值给 x，再删除 w 节点即可
   2. 沿着右子树的左链走到尽头的节点 w 的元素是 x 右子树里最小的元素\
      只需将 w 元素赋值给 x，再删除 w 节点即可

```c++
template <class Record>
Error_code
Search_tree<Record> :: search_and_destroy
(Binary_node<Record>*&root1, const Record &target)
{
    if (root1 ** NULL || root1->data ** target)return remove_root(root1);
    if (target < root1->data)return search_and_destroy(root1->left, target);
    return search_and_destroy(root1->right, target);
}
```

```c++
template <class Record>
Error_code
Search_tree<Record> :: remove_root
(Binary_node<Record>*&root1)
{
    if (root1 ** NULL) return not_present;
    Binary_node<Record> *to_delete = root1;
    if (root1->right ** NULL) root1 = root1->left;
    else if (root1->left ** NULL) root1 = root1->right;
    else
    {
        to_delete = root1->left;
        Binary_node<Record> *parent= root1;
        while (to_delete->right != NULL)
        {
            parent = to_delete;
            to_delete = to_delete->right;
        }
        root1->data = to_delete->data;
        if (parent = root1) root1->left = to_delete->left; //重点
        else parent->right = to_delete->left; //重点
    }
    delete to_delete; return success;
}
```

### 平衡二叉树

- 二叉树结构会影响**查找**效率

  - 二叉树结构越趋向于完全二叉树则时间复杂度越接近$O(log(n))$，效率越好

  - 二叉树结构越趋向于线性链表则时间复杂度越接近于$O(n)$，效率越差

    - 最坏的情况会退化为$O(n)$的线性查找复杂度

平均访问节点次数：$2ln(n)\approx1.39log(n)$

平均比较节点次数：$4ln(n)\approx2.77log(n)$

也就是为什么我们需要**自平衡二叉查找树**

#### AVL Trees

AVL 是一种平衡二叉树(自平衡二叉查找树)，过于复杂，没有太大优势，实际更常用的是红黑树来实现

##### 平衡因子 BF(Balance Factor)

$BF=左子树深度-右子树深度$

##### AVL 定义

任意节点的 BF 只能为-1、0 或 1

##### 插入元素

插入元素会改变 BF，需要调整来维持二叉树的平衡

AVL 树采用四种“旋转”调整操作来实现

##### 代码实现：

https://blog.csdn.net/weixin_57761086/article/details/127152576

此处定义：$BF=右子树深度-左子树深度$

###### 数据结构定义

```c++
//结点类
template <typename T>
struct AVLTreeNode {
	AVLTreeNode<T>* left;//左孩子指针
	AVLTreeNode<T>* right;//右孩子指针
	AVLTreeNode<T>* parent;//双亲指针
	T value;//存储元素的变量
	int bf;//平衡因子

	//用传入的值构造结点
	AVLTreeNode(const T& _value = T())
		:left(nullptr)
		,right(nullptr)
		,parent(nullptr)
		,value(_value)
		,bf(0)
	{}
};
```

```c++
//AVL树类
template<typename T>
class AVLTree {
	//给结点类起别名，方便使用
	typedef AVLTreeNode<T> Node;
private:
	//指向根结点的结点指针
	Node* root;
};
```

```c++
//无参构造
//让根结点指针指向空即可
AVLTree():root(nullptr){}
```

###### 左旋转（RR）

当新节点插入到较高右子树的右侧时(右右/RR)，需要对最先失去平衡的节点进行一次左旋，使其右子树成为新的根节点，原根节点成为其左子树。

```c++
//左单旋
void RotateLeft(Node* parent) {
	//这里的指针设计在讲解旋转的时候已经解释过
	Node* subR = parent->right;
	Node* subRL = subR->left;

	//用pparent(祖父结点)来保存parent结点的双亲
	//因为parent并不一定是整棵树的根，还有可能只是一棵子树的根
	Node* pparent = parent->parent;

	//第一步：首先让双亲的右孩子指针指向subRL
	parent->right = subRL;

	//之所以要判断是因为subRL结点可能不存在
	if (subRL)
		//更新双亲
		subRL->parent = parent;

	//第二步：让subR的左孩子指针指向parent
	subR->left = parent;

	//不要忘记给这些结点更新它们的双亲
	parent->parent = subR;
	subR->parent = pparent;

	//祖父结点是空，说明parent结点是整棵树的根结点
	if (pparent ** nullptr) {
		root = subR;
	}
	//说明parent是子树的根结点
	else {
		//原本祖父结点的孩子是parent
		//经过旋转他的孩子变成了subR
		//需要判断parent是祖父结点的左孩子还是右孩子，然后让对应指针指向subR结点
		if (pparent->left ** parent) {
			pparent->left = subR;
		}
		else {
			pparent->right = subR;
		}
	}
	//更新平衡因子
	parent->bf = subR->bf = 0;
}
```

###### 右旋转（LL）

当新节点插入到较高左子树的左侧时(左左/LL)，需要对最先失去平衡的节点进行一次右旋，使其左子树成为新的根节点，原根节点成为其右子树。

```c++
//右单旋
void RotateRight(Node* parent) {
	Node* subL = parent->left;
	Node* subLR = subL->right;

	//祖父结点
	Node* pparent = parent->parent;

	//第一步：让parent的左孩子指针指向subLR
	parent->left = subLR;
	//之所以要判断是因为subLR结点可能不存在
	if (subLR)
		subLR->parent = parent;
	//第二步：让subL的右孩子指针指向parent
	subL->right = parent;

	//更新双亲结点
	parent->parent = subL;
	subL->parent = pparent;

	//祖父结点是空，说明parent是整棵树的根结点
	if (pparent ** nullptr) {
		root = subL;
	}
	//parent是子树的根结点
	else {
		//这里和左单旋一样
		if (pparent->left ** parent) {
			pparent->left = subL;
		}
		else {
			pparent->right = subL;
		}
	}
	//更新平衡因子
	parent->bf = subL->bf = 0;
}
```

###### 左右双旋（LR）

当新节点插入到较高左子树的右侧时(左右/LR)，需要先对左子树进行一次左旋，这时就变为 RR 情形，再对最先失去平衡的节点进行一次右旋，使其左子树的右子树成为新的根节点。

```c++
//左右双旋
void RotateLR(Node* parent) {
	Node* subL = parent->left;
	Node* subLR = subL->right;
	//保存subLR的平衡因子，后面需要根据它来更新别的平衡因子
	int BF = subLR->bf;

	//对以subL为根结点的树进行左单旋
	RotateLeft(subL);
	//对以parent为根结点的树进行右单旋
	RotateRight(parent);

	//根据保存的平衡因子更新
	//当subLR平衡因子为-1，将parent的平衡因子更新为1
	if (BF ** -1) parent->bf = 1;
	//当subLR平衡因子不是-1，将subL的平衡因子更新为-1
	else subL->bf = -1;
}
```

###### 右左双旋（RL）

当新节点插入到较高右子树的左侧时(右左/RL)，需要先对右子树进行一次右旋，这时就变为 LL 情形，再对最先失去平衡的节点进行一次左旋，使其右子树的左子树成为新的根节点。

```c++
//右左双旋
void RotateRL(Node* parent) {
	Node* subR = parent->right;
	Node* subRL = subR->left;
	//保存subRL的平衡因子，后面根据它更新其他结点平衡因子
	int BF = subRL->bf;

	//对以subR为根的树进行右单旋
	RotateRight(subR);
	//对以parent为根的树进行左单旋
	RotateLeft(parent);

	//根据保存的subRL平衡因子的不同来进行不同的更新。
	if (BF ** -1) subR->bf = 1;
	else parent->bf = -1;
}
```

###### 插入过程

```c++
//插入函数
bool Insert(const T& _value) {
	//树为空，将新结点作为根结点即可
	if (root ** nullptr) {
		root = new Node(_value);
		return true;
	}

	//树非空
	Node* cur = root;
	//用来保存cur的双亲结点
	Node* parent = nullptr;

	//为新结点的插入寻找合适的位置
	//同时判断是否树中有相同结点
	while (cur) {
		parent = cur;
		if (_value < cur->value) {
			cur = cur->left;
		}
		else if (_value > cur->value) {
			cur = cur->right;
		}
		else {
			return false;
		}
	}

	//构造新结点
	cur = new Node(_value);
	//判断是新结点应该插在双亲的左边还是右边
	if (_value < parent->value) {
		parent->left = cur;
	}
	else {
		parent->right = cur;
	}
	//更新新结点的双亲
	cur->parent = parent;

	//更新平衡因子
	//从下往上
	while (parent) {
		//cur是双亲左孩子
		//左子树高度变高，平衡因子就减1
		//平衡因子：右子树高度减左子树高度
		if (cur ** parent->left) {
			parent->bf--;
		}
		//右子树变高，平衡因子加1
		else {
			parent->bf++;
		}

		//判断更新后的平衡因子是否满足AVL树
		//证明parent之前只有一个孩子，而新结点插到另一个空着的位置了
		if (parent->bf ** 0) {
			break;
		}
		//证明parent之前没有孩子，新结点插入后只有一个孩子
		else if (-1 ** parent->bf || 1 ** parent->bf) {
			cur = parent;
			parent = cur->parent;
		}
		//如下就是parent的平衡因子变成2或-2了
		//需要根据图片进行理解
		else {
			//说明parent的右子树高
			if (parent->bf ** 2) {
				//根据cur的平衡因子判断是左单旋还是右左双旋
				if (cur->bf ** 1)
					RotateLeft(parent);
				else
					RotateRL(parent);
			}
			//parent左子树高
			else {
				if (cur->bf ** -1)
					RotateRight(parent);
				else
					RotateLR(parent);
			}

			//使用双旋调整后就不需要向上继续调整了，可以直接退出循环
			break;
		}
	}
	return true;
}
```
