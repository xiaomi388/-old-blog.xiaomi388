---
layout: post
title: 515. Find Largest Value in Each Tree Row
tags: [LeetCode]
---

*source:[LeetCode][1]*


{% highlight C++ linenos %}
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
    private:
        vector<int > vpq;

        void dfs(TreeNode* now, int row)
        {
            if (now == NULL) return;
            //检测是否越界
            if (row >= vpq.size())
                vpq.push_back(pq);
            vpq[row] = max(now->val, vpq[row]);
            //dfs
            dfs(now->left, row+1);
            dfs(now->right, row+1);
        }

    public:
        vector<int> largestValues(TreeNode* root) {
            dfs(root, 0);
            return vpq;
        }
}
{% endhighlight %}


[1]:	https://leetcode.com/problems/find-largest-value-in-each-tree-row/#/description
