---
layout: post
title: 算法基础笔记
tags: [Learning]
---
## 第八周

### 贪心算法

#### 问题1：
n个会议要使用一个会议室，每个会议需要使用一定时间段（开始时刻，结束时刻），问在该会议室安排最多会议的方法。

##### 作用
梦秋说：求重叠最多/最少

##### 算法
结束时间最早的会议优先。

##### 算法正确性判断
1. 定理是安全的
2. 问题具有最优子结构（整个最优，局部肯定也最优）
	- 整条最短路，则局部也是最短路。

#### 并查集

	#include<iostream>
	#include<cstring>
	#include<cstdio>
	#include<cstdlib>
	using namespace std;
	
	int father[50002],a,b,m,n,p;
	int find(int x){
	if(father[x]!=x)
	father[x]=find(father[x]);
	/*
	x代表例题中的人，father[x]中所存的数代表这一集合中所有人都与一个人有亲戚关系
	相当于例题中第一个集合所有的元素都与第一个元素有亲戚关系
	搜索时只要找元素所指向的father[x]=x的元素(即父元素)
	然后比较两个元素的父元素是否相同就可以判断其关系
	*/
	return father[x];
	}
	int main()
	{
	  int i;
	  scanf("%d%d%d",&n,&m,&p);
	  for(i=1;i<=n;i++)
	    father[i]=i;
	  for(i=1;i<=m;i++)
	    {
	      scanf("%d%d",&a,&b);
	      a=find(a);
	      b=find(b);
	      father[a]=b;
	    }
	  for(i=1;i<=p;i++)
	    {
	      scanf("%d%d",&a,&b);
	      a=find(a);
	      b=find(b);
	      if(a==b)
	        printf("Yes\n");
	      else
	        printf("No\n");
	    }
	  return 0;
	}

#### 割性质

#### 均摊分析

#### 并查集的数据结构(克里斯托算法）(O(E))
	procedure makeset(x)
		π(x) = x
		rank(x) = 0
	
	function find(x)
		while x != π(x) : x = π(x)
		return x;
	 
	function union(x,y)
		rx = find(x)
		ry = find(y)
	if rx=ry: return
	if rank(rx) > rank(ry):
		π(ry) = rx
	else
		π(rx) = ry
		if rank(rx) = rank(ry) : rank(ry) = rank(ry) + 1

#### 路径压缩(每次查找操作中，顺便改变路上的节点的父指针使其直接指向根）
	function find(x)
		if x!= π(x) : π(x) = find(π(x))
		return π(x)


#### 三个性质
1. 任意x，等级 \< 父辈等级
2. 任一根节点等级为k的树至少包含2^k个节点
3. 如果共有n个元素，则最多有n/2^k个等级为k的节点

#### 证明路径压缩降低复杂度
- max find: n log\*n
- 初始给定每个节点mlog\*n元
- find操作花钱
- find操作需要O(log\*n)步
- 某个节点不为根时，收到上述的补贴

### 普林算法(O(V^2))
	cost(v) = min w(u,v)
	for all u in v:
		cost(u) = INFINITE
		prev(u) = nil
	
	pick any initial node u0
	cost(u0) = 0
	 
	H = makequeue(V)(优先级队列，以cost为键
	while H is not empty:
		v = deletemin(H)
		for each {v,z} in E
		if cost(z)>w(v,z)
			cost(z) = w(v,z)
			prev(z) = v
