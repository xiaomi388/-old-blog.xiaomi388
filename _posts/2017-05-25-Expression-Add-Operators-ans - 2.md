---
layout: post
title: 282. Expression Add Operators
tags: [LeetCode]
---

*source:[LeetCode][1]*


{% highlight C++ linenos %}
class Solution {
	private:
	    // cur: {string} expression generated so far.
	    // pos: {int}    current visiting postion of num
	    // cv:  {long}   cumulative value so far
	    // pv:  {long}   previous operand value
	    // op:  {char}   previous operator used.
	    void dfs(vector<string>& res, const string& num, const int target, string cur, int pos, const long cv, const long pv, const char op)
	    {
	        if (pos == num.size() && cv == target)
	            res.push_back(cur);
	        else
	        {
	            for (int i=pos+1; i<=num.size(); i++)
	            {
	                string t = num.substr(pos, i-pos);
	                long now = stol(t);
	                if (to_string(now).size() != t.size()) continue;
	                dfs(res, num, target, cur+'+'+t, i, cv+now, now, '+');
	                dfs(res, num, target, cur+'-'+t, i, cv-now, now, '-');
	                dfs(res, num, target, cur+'*'+t, i, (op == '-') ? cv+pv - pv*now : ((op == '+') ? cv-pv + pv*now : pv*now), pv*now, op);
	            }
	        }
	    }
	
	public:
	    vector<string> addOperators(string num, int target)
	    {
	        vector<string> res;
	        if (num.empty()) return res;
	        for (int i=1; i<=num.size(); i++)
	        {
	            string s = num.substr(0, i);
	            long cur = stol(s);
	            if (to_string(cur).size() != s.size()) continue;
	            dfs(res, num, target, s, i, cur, cur, '#');
	        }
	    return res;
	    }
};

/\* summary
 * 1. 遍历所有情况，使用引用传递来传递答案，遇到可行的方案时，直接push答案到res中。
 * 2. to\_string 和 stol 的用法。要更好地掌握 cchar 和 string 的使用。
 * 3. dfs：一个一个地试，去遍历
 \*

{% endhighlight %}


[1]:	https://leetcode.com/problems/expression-add-operators/#/description