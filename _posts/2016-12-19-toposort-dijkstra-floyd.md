---
layout: post
title: 拓扑排序、Dijkstra、Floyd算法、AVL树的旋转
tags: [Code]
---
## 拓扑排序

作用：将有向图中的顶点以线性方式进行排序。即对于任何连接自顶点u到顶点v的有向边uv，在最后的排序结果中，顶点u总是在顶点v的前面。

形象例子:选课——再选某些课之前必须先选另外的某些基础课，确定选课顺序。

伪代码实现如下：

Kahn算法实现：

	L← Empty list that will contain the sorted elements
	S ← Set of all nodes with no incoming edges
	
	while S is non-empty do
	    remove a node n from S
	    insert n into L
	    foreach node m with an edge e from n to m do
	        remove edge e from the graph
	        if m has no other incoming edges then
	            insert m into S
	if graph has edges then
	    return error (graph has at least onecycle)
	else 
	    return L (a topologically sortedorder)

DFS实现：

	L ← Empty list that will contain the sorted nodes
	S ← Set of all nodes with no outgoing edges
	for each node n in S do
	    visit(n) 
	function visit(node n)
	    if n has not been visited yet then
	        mark n as visited
	        for each node m with an edge from m to n do
	            visit(m)
	        add n to L

## Floyd算法
作用：算出所有顶点到其余各顶点的最短路径算法。
很好的一篇教程[坐在马桶上看算法：只有五行的Floyd最短路径算法][1]

## 迪杰斯特拉算法
原理图如下：
![][image-1]

## AVL树的旋转
[数据结构：关于AVL树的平衡旋转详解][2]

[1]:	http://developer.51cto.com/art/201403/433874.htm
[2]:	http://blog.csdn.net/lemon_tree12138/article/details/50393548 "数据结构：关于AVL树的平衡旋转详解"

[image-1]:	/public/img/Dijkstra.gif