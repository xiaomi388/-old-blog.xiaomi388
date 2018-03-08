---
layout: post
title: 486. Predict the Winner
tags: [LeetCode]
---

*source:[LeetCode][1]*

#### 起始思路
动态规划，新开数组存储起始点为i，长度为q时玩家1比玩家2多赢的分数。

#### 二维数组实现
{% highlight C++ linenos %}
class Solution {
public:
	bool PredictTheWinner(vector<int>& nums) {
	    int is_winner[nums.size()+5][nums.size()+5]; // 1维表示起始，2维表示长度
	    memset(is_winner,0,sizeof is_winner);
	    for(int i=1;i<=nums.size();i++)
	        is_winner[i][1] = nums[i-1];
	    for(int i=2;i<=nums.size();i++) //i 表示长度
	        for(int q=1;q<=nums.size()-i+1;q++) // q表示起始
	            is_winner[q][i] = max(-is_winner[q][i-1]+nums[q+i-2],-is_winner[q+1][i-1]+nums[q-1]);
	    return is_winner[1][nums.size()]>=0;
	}
}
{% endhighlight %}

#### 一维数组实现
{% highlight C++ linenos %}
class Solution {
public:
	bool PredictTheWinner(vector<int>& nums) {
	    int is_winner[nums.size()+5]; // 1维表示起始，2维表示长度
	    memset(is_winner,0,sizeof is_winner);
	    for(int i=1;i<=nums.size();i++)
	        is_winner[i] = nums[i-1];
	    for(int i=2;i<=nums.size();i++) //i 表示长度
	        for(int q=1;q<=nums.size()-i+1;q++) // q表示起始
	            is_winner[q] = max(-is_winner[q]+nums[q+i-2],-is_winner[q+1]+nums[q-1]);
	    return is_winner[1]>=0;
	}
};
{% endhighlight %}

[1]:	https://leetcode.com/problems/predict-the-winner/#/description
