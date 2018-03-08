---
layout: post
title: 495.Teemo Attacking
tags: [LeetCode]
---

*source:[LeetCode][1]*

{% highlight C++ linenos %}
class Solution {
public:
	int findPoisonedDuration(vector<int>& timeSeries, int duration) {
	    if(timeSeries.empty() || duration == 0)return 0;
	    int i = 0;
	    int time_rec = 0;
	    int expect_time = 0;
	    while(i < timeSeries.size())
	    {
	        int now_time = timeSeries[i];
	        expect_time = now_time + duration;
	        while(expect_time > timeSeries[i] && i<timeSeries.size())
	        {
	            expect_time = timeSeries[i] + duration;
	            i++;
	        }
	        time_rec += timeSeries[i-1] - now_time + duration;
	    }
	    return time_rec;
	}
};
{% endhighlight %}

#### 总结
开始的时候以秒为单位，模拟全过程，后超时。

 

[1]:	https://leetcode.com/problems/teemo-attacking/?tab=Description