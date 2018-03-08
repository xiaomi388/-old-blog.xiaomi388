---
layout: post
title: 452. Minimum Number of Arrows to Burst Balloons
tags: [LeetCode]
---

*source:[LeetCode][1]*

#### 起始思路
此题与课上讲的开会的例子方法一样，给出一个时间的起始点和终止点，只不过开会的例子是要求出不重叠的，但这道题要求重叠最多的。因此可以模仿开会的例子的解题方法，先按照终止点从小到大排序（如果终止点相同则比较起始点），然后先在排序后的第一个气球终止点射一支箭，对于剩余的所有气球，如果起始点不在上一支箭的位置之后（即上一支箭已经将这个气球射破），则跳过这个气球；否则在这个气球的终止点再射一支箭，箭的总数加一。


{% highlight C++ linenos %}
bool comp(pair<int,int> a,pair<int,int> b)
{
    return a.second == b.second?a.first<b.first : a.second<b.second;
}

class Solution {
public:
    vector<pair<int,int> > set;
    int findMinArrowShots(vector<pair<int, int> >& points) {
        sort(points.begin(),points.end());
        for(auto point : points)
        {
            bool left = 0; // 0 for no, 1 for yes
            bool right = 0;
            for(int i=0;i<set.size();i++)
            {
                pair<int,int> * now = &set[i];
                if(point.first >= now->first && point.first <= now->second)
                    left = 1;
                if(point.second >= now->first && point.second <= now->second)
                    right = 1;
                if(left || right)
                {
                    if(left)now->first = point.first;
                    if(right)now->second = point.second;
                    break;
                }
            }
            if(!left && !right)
                set.push_back(point);
        }
        return set.size();
    }
}
{% endhighlight %}
 

#### 大神代码

 
{% highlight C++ linenos %}
class Solution {  
public:  
    int findMinArrowShots(vector<pair<int, int>>& points) {  
        if(points.size() == 0)  
            return 0;  
        sort(points.begin(), points.end(), comp);  
        int count = 1;  
        int arrow = points[0].second;  
        for(int i = 1; i < points.size(); i ++){  
            if(points[i].first <= arrow)  
                continue;  
            else{  
                count ++;  
                arrow = points[i].second;  
            }  
        }  
        return count;  
    }  
  
    static bool comp(pair<int, int>& a, pair<int, int>& b){  
        return a.second == b.second ? a.first < b.first : a.second < b.second;  
    }  
};  
{% endhighlight %}

#### 大神代码分析

[1]:	https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/#/description
