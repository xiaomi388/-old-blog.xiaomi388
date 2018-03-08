---
layout: post
title: 2.Add Two Numbers
tags: [LeetCode]
---

*source:[LeetCode][1]*

{% highlight C++ linenos %}
class Solution {
public:
	ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
	    ListNode * now_1 = l1;
	    ListNode * now_2 = l2;
	    ListNode * result = new ListNode(0);
	    ListNode * first = result;
	    ListNode * last;
	    int C = 0;
	    while(now_1 != NULL && now_2 != NULL)
	    {
	        result->val = (now_1->val + now_2->val + C) % 10;
	        if(now_1->val + now_2->val + C >= 10)
	            C = 1;
	        else
	            C = 0;
	        now_1 = now_1->next;
	        now_2 = now_2->next;
	        ListNode * next_result = new ListNode(0);
	        result->next = next_result;
	        last = result;
	        result = next_result;
	    }
	    while(now_1 != NULL)
	    {
	        result->val = (now_1->val + C) % 10;
	        if(now_1->val + C >= 10)
	            C = 1;
	        else
	            C = 0;
	        now_1 = now_1->next;
	        ListNode * next_result = new ListNode(0);
	        result->next = next_result;
	        last = result;
	        result = next_result;
	    }
	    while(now_2 != NULL)
	    {
	        result->val = (now_2->val + C) % 10;
	        if(now_2->val + C >= 10)
	            C = 1;
	        else
	            C = 0;
	        now_2 = now_2->next;
	        ListNode * next_result = new ListNode(0);
	        result->next = next_result;
	        last = result;
	        result = next_result;
	    }
	    if(C)
	    {
	        result->val = C;
	        C = 0;
	    }
	    else
	        last->next = NULL ;
	    return first;
	}
};
{% endhighlight %}
 

#### 总结
开始的时候没有考虑两个很大数字相加的情况，于是用了先将两个链表转换成 int 后相加再转链表的方法，代码如下：

 
{% highlight C++ linenos %}
class Solution {
public:
	ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
	    ListNode * now_1 = l1;
	    ListNode * now_2 = l2;
	    int l1_int = 0;
	    int l2_int = 0;
	    int weight = 1;
	    while(now_1 != NULL)
	    {
	        l1_int += now_1->val*weight;
	        weight *= 10;
	        now_1 = now_1->next;
	    }
	    weight = 1;
	    while(now_2 != NULL)
	    {
	        l2_int += now_2->val*weight;
	        weight *= 10;
	        now_2 = now_2->next;
	    }
	    int result_int = l1_int + l2_int;
	    ListNode * result = new ListNode(result_int % 10);
	    ListNode * first = result;
	    result_int /= 10;
	    while(result_int != 0)
	    {
	        result->next = new ListNode(result_int %10);
	        result_int /= 10;
	        result = result->next;
	    }
	    return first;
	}
};
{% endhighlight %}

果不其然，WA。因此，在选择方法去做题的时候，要考虑简单方法能否通过所有可能的情况。

## 修改后的代码

{% highlight C++ linenos %}
/** 
 * Definition for singly-linked list.
 * struct ListNode {
 * int val;
 * ListNode *next;
 * ListNode(int x) : val(x), next(NULL) {}
 * };
 */
 
class Solution {
public:
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
ListNode* left = l1, * right = l2;
ListNode* ans = new ListNode(0);
ListNode* now = ans;
int flag = 0; // ji lu jin wei
while (left || right || flag)
{
int sum = (left ? left-\>val : 0) + (right ? right-\>val : 0) + flag;
flag = sum / 10;
//lianjie
now-\>next = new ListNode(sum % 10);
now = now-\>next;
left = left ? left-\>next : NULL;
right = right ? right-\>next : NULL;
}
return ans-\>next;
   
}
};
{% endhighlight %}

[1]:	https://leetcode.com/problems/add-two-numbers/?tab=Description