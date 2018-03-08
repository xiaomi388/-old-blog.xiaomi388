---
layout: post
title: 546. Remove Boxes
tags: [LeetCode]
---

*source:[LeetCode][1]*


#### 起始思路
此题难度较大，在久经思考无果后，最终参考了一下大神的思路，后来经过整理才得出答案。
1. 首先对于此类多次分割的问题，我们可以确定，对于任意串，最后的连续相同的盒子（设第一个为r)必然要取在同一组。然后进行深搜，找出该序列前面与最后一个盒子颜色相同的盒子序号i，并先分割i+1...r-1，而后把1...i+最后的盒子分为一组。通过这种方法进行递归求解。
2. 因此，我们需要一个三维数组进行存储：第一维含义是起始点，第二维含义是终止点，第三维则是在某个序列之后紧跟着k个和该序列最后一个元素相同的点。

#### 记忆化搜索代码
{% highlight C++ linenos %}
class Solution{
public:
	int removeBoxes(vector<int>& boxes)
	{
	    int n = boxes.size();
	    int memo[100][100][100] = {0};
	    return dfs(boxes,memo,0,n-1,0);
	}
	
	int dfs(vector<int>& boxes,int memo[100][100][100],int l,int r,int k)
	{
	    if(l>r) return 0;
	    if(memo[l][r][k]!=0)return memo[l][r][k];
	
	    while(r>l && boxes[r]==boxes[r-1])
	        r--;k++;
	    memo[l][r][k] = dfs(boxes,memo,l,r-1,0) + (k+1)*(k+1);
	    for(int i=l;i<r;i++)
	    {
	        if(boxes[i] == boxes[r])
	            memo[l][r][k] = max(memo[l][r][k],dfs(boxes,memo,l,i,k+1)+dfs(boxes,memo,i+1,r-1,0);
	    }
	    return memo[l][r][k];
	}
}

{% endhighlight %}

#### 为什么不使用递推
因为涉及到了三维，边界条件较难构造，因此使用记忆化搜索会相对方便些。


[1]:	https://leetcode.com/problems/remove-boxes/#/description