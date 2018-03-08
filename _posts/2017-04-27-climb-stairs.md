---
layout: post
title: 70. Climbing Stairs
tags: [LeetCode]
---

*source:[LeetCode][1]*

#### 起始思路
动态规划。

{% highlight C++ linenos %}
class Solution {
public:
	int climbStairs(int n) {
	    int first = 1;
	    int second = 2;
	    for(int i=3;i<=n;i++)
	    {
	        int tmp = first + second;
	        first = second;
	        second = tmp;
	    }
	    return n==1?first:second;
	}
};
{% endhighlight %}
 

[1]:	https://leetcode.com/problems/climbing-stairs/#/description