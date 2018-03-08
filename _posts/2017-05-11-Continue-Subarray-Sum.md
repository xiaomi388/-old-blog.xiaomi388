---
layout: post
title: 523. Continuous Subarray Sum
tags: [LeetCode]
---

*source:[LeetCode][1]*

### 方法一

#### 起始思路
动态规划，新开数组存储起始点为i，长度为q时的包含符合条件的子序列，转移方程复杂度o(n)，总复杂度O(n^2)

#### 代码
{% highlight C++ linenos %}
class Solution {
	public:
	    bool checkSubarraySum(vector<int>& nums, int k) {
	        bool check[nums.size()+3]; //start,length.
	        if(nums.size()==1)return false;
	        for(int i=1;i<nums.size();i++)
	        {
	            if(k!=0)
	            {
	                check[i] = ((nums[i-1]+nums[i]) % k == 0);
	                if(nums[i-1]+nums[i] == 0)return true;
	            }
	            else
	            {
	                if(nums[i-1]+nums[i] == 0)return true;
	                else return false;
	            }   
	        }
	        for(int i=3;i<=nums.size();i++)//length
	            for(int q=1;q<=nums.size()-i+1;q++)//start
	            {
	                check[q] = check[q] || check[q+1];
	                if(!check[q])
	                {
	                    int sum = 0; 
	                    for(int j=q-1;j<q-1+i;j++)sum+=nums[j];
	                    check[q] = (sum%k == 0);
	                }
	            }
	        return check[1];
	    }
};


{% endhighlight %}


#### 缺点
耗时太长，1863ms通过。

### 方法二

#### 起始思路
二维数组：存储起始和终止的子序列模k的结果。转移复杂度O(1），总复杂度O(n)

#### 代码
{% highlight C++ linenos %}
class Solution {
	public:
	    bool checkSubarraySum(vector<int>& nums, int k) {
	        if(k == 0)
	        {
	            for(int i=0;i<nums.size()-1;i++)
	                if(nums[i] + nums[i+1] == 0)return true;
	            return false;
	        }
	        int check[nums.size()+3];
	        for(int i=1;i<=nums.size();i++)
	            check[i] = nums[i-1];
	        for(int i=2;i<=nums.size();i++) //end point
	            for(int q=1;q<i;q++) //start
	            {
	                check[q] = (check[q] + nums[i-1]) % k;
	                if(check[q] == 0)
	                    return true;
	            }
	        return false;
	    }
};
{% endhighlight %}

[1]:	https://leetcode.com/problems/continuous-subarray-sum/#/description