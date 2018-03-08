---
layout: post
title: 000. 关于链表的一些题
tags: [LeetCode]
---

{% highlight C++ linenos %}
#include<cstdio>
struct Node
{
    int val;
    Node * next;
    Node(int x) : val(x), next(NULL){}
};

// 如何判断一个单链表是否有环？
// 快慢指针法
bool is_exits_loop(Node * head)
{
    Node* slow = head, *fast = head;

    while ( fast && fast->next )
    {
        slow = slow->next;
        fast = fast->next->next;
        if ( slow == fast ) break;
    }

    return !(fast == NULL || fast->next == NULL);
}

// 判断环长度
// 从碰撞点开始，再次碰撞所走过的就是环的长度：因为fast恰好快slow一圈
int FindLoopDistance(Node* head)
{
    Node * slow = head, *fast = head;

    while ( fast && fast->next )
    {
        slow = slow->next;
        fast = fast->next->next;
        if ( slow == fast ) break;
    }

    if (fast == NULL || fast->next == NULL)
        return NULL;

    int dist = 0;
    while (1)
    {
        dist++;
        slow = slow->next;
        fast = fast->next->next;
        if ( slow == fast ) break;
    }

    return dist;
}

// 寻找环连接点
// 定理：碰撞点 p 到连接点的距离=头指针到连接点的距离
// 因此，分别从碰撞点、头指针开始走，相遇的那个点就是连接点。
Node* FindLoopPort(Node* head)
{
    Node * slow = head, *fast = head;

    while ( fast && fast->next )
    {
        slow = slow->next;
        fast = fast->next->next;
        if ( slow == fast ) break;
    }

    if (fast == NULL || fast->next == NULL)
        return NULL;

    slow = head;
    while (slow != fast)
    {
        slow = slow->next;
        fast = fast->next;
    }

    return slow;
}

//带环链表的长度
//问题3中已经求出连接点距离头指针的长度，加上问题2中求出的环的长度，二者之和就是带环单链表的长度
int main()
{
    Node head(1), a(2), b(3), c(4), e(5);
    head.next = &a;
    a.next = &b;
    b.next = &c;
    c.next = &head;
    printf("%d", FindLoopDistance(&head));
}
{% endhighlight %}
