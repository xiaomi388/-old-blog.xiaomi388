---
layout: post
title: 402. Remove K Digits
tags: [LeetCode]
---

*source:[LeetCode][1]*

#### 起始思路
贪心算法：抽出K个数字，即说明在num中取出num.length() - K 个数字，使其构成一个最小的数。那么我们先取第一位，第一位数只能在 1-(K+1)个数字中取，第二个在（第一个数）-（K+2）个数字中取，以此类推，每次选出最小数字即可。

{% highlight C++ linenos %}
# include<iostream>
using namespace std;

class Solution {
public:

	int min(int a,int b)
	{
	    return a<b?a:b;
	}
	
	string removeKdigits(string num, int k) {
	    string result;
	    result.clear();
	    int total_take = num.length() - k;
	    int last_pos = -1; 
	    for(int q=0;q<total_take;q++)
	    {
	        int min_num = 100;
	        for(int i=last_pos+1;i<=k+q;i++)
	        {
	            if(num[i] < min_num)
	            {
	                min_num = num[i];
	                last_pos = i;
	            }
	        }
	        if((result.length()==0) && (num[last_pos] == '0'))
	            int a = 0;
	        else
	            result += num[last_pos];
	    }
	    if(result.empty())
	        result += '0';
	    return result;
	}
};


{% endhighlight %}
 

[1]:	https://leetcode.com/problems/remove-k-digits/#/description