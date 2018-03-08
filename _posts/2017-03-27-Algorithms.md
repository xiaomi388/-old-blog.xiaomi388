---
layout: post
title: 算法基础笔记
tags: [Learning]
---
### 第五周

##### 强连通分量

##### 求强连通分量（Kosaraju算法）
1. 对有向图取逆，得到G的反向图￼￼￼￼￼￼￼￼G^R
2. 利用DFS求出G^R的逆后排序
3. 对G按照上述逆后排序的序列进行DFS
4. 对同一个DFS递归子程序中访问的所有顶点都在同一个强连通分量内

### 第六周：图中的路径

#### BFS

##### BFS求最短路径

	for all u in V
		dist(u) = MAX_DISTANCE // 初始化所有点距离为无穷，代表没有访问
	
	dist(s) = 0
	Q = [s] // push 初始点
	while Q is not empty:
		u = eject(Q) //取出队列头且去掉
		for all edges(u,v) in E
			if dist(v) = MAX_DISTANCE //无穷距离代表没有访问
				inject(Q,v)
				dist(v) = dist(u) +1

时间复杂度O(V+E)

##### 隐式图的BFS

> 隐式图：问题中没有明确给出图结构，图需要我们一边BFS一边构造

- 例子：八数码问题

	> 将任意摆放的数码盘以最少的步数摆成某种特殊的排列

- 例子：农夫过河问题

	> 农夫带狼、羊、菜过河，每次只能带一个，求农夫如何过河

	对人狼羊菜，0表示没过河，1表示过河。
	(0,0,0,0) -> (1,0,1,0)?1 -> (0,0,1,0) -> (1,1,1,0)?0 -> (1,0,1,1)?1 -> ... -> (1,1,1,1)

##### 带权图的单源最短路

###### 方法1
在边中插入节点，长度为w的边插入w-1个节点，转化为无权图最短路问题。
蚂蚁、闹钟模型

###### Dijkstra算法

	求：从 s 到 u 的最短距离
	for all u in V:
		dist(u) = MAX_DISTANCE
		prev(u) = nil    // 记录最短路径是怎么走出来的，返回上一个点
	dist(s) = 0
	
	H = makequeue(V) //  using dist-values as keys
	while H is not empty:
		u = deletemin(H)  // 模拟最早响的闹钟
		for all edges (u,v) in E:
			if  dist(u) + l(u,v) <= dist(v)  // s->u->v 的距离 小于 s->v 的距离
				dist(v) = dist(u) + l(u,v)
				prev(v) = u
				decreasekey(H,v) //减小队列里的dist值

使用的数据结构：
- 数组 O(V^2)
- *最小堆* O((V+E)logV)

##### 求负权图最短路

> 如果图中含有负权边， Dijkstra 算法无法求得正确的最短路 -\> 若存在负圈则无效。

###### Update操作

	dist(v) = min{dist(v),dist(u) + l(u,v)}

###### Bellman-Ford算法
	for all u in V:
		dist(u) = MAX
		prev(u) = NULL
	
	dist(s) = 0
	repeat V-1 times
		for all e in E
			update(e)
O(VE)

###### 利用队列优化
	for all u in V
		dist(u) = MAX
	dist(s) = 0
	enque(Q,s)
	while Q is not empty:
		u = deque(Q)
	for each e(u,v):
		update(e)
		if dist(v) becomes smaller and v is not in Q
		enque(Q,v)