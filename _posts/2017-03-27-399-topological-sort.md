---
layout: post
title: 207.Course Schedule
tags: [LeetCode]
---

*source:[LeetCode][1]*

#### 难点
拓扑排序

#### 起始思路
用 DFS 判断是否存在回边。由于先前不懂得如何根据 pre 和 post 判断该边是否回边，因此在该过程中遇到了许多问题。

#### 代码如下


{% highlight C++ linenos %}
# include<iostream>
# include<algorithm>
# include<stack>
# include<cstdio>
# include<vector>

using namespace std;
struct Vertex
{
	int id;
	int pre;
	int post;
	vector<struct Edge*> in_edges;
	vector<struct Edge*> out_edges;
};

struct Edge
{
	struct Vertex * next_ver;
	struct Vertex * last_ver;
};


class Solution {
public:
	int clock;
	/*
	bool comp(Vertex a,Vertex b)
	{
	    return a.post<b.post?a:b;
	}
	*/
	
	bool check(int numCourses,struct Vertex * vers)
	{
	    int rec = 0;
	    cout << vers[0].in_edges.size() << endl;
	    stack<Vertex *> s;
	    for(int i=0;i<numCourses;i++)
	    {
	        //寻找入度为0的点
	        if(vers[i].in_edges.size()==0)
	        {
	            s.push(&vers[i]);
//                cout \<\< vers[i].id \<\<  "dsa" \<\<i \<\< endl;
	        }
	    }
	    //DFS
	    while(!s.empty())
	    {
	        clock++;
	        struct Vertex * now = s.top();
	        cout << "now id: " << now->id << endl;
	        //如果之前没有visit，则置pre
	        if(now->pre == -1)
	            now->pre = clock;
	        bool no_out = 1;
	        for(auto out : now->out_edges)
	        {
	            //如果没有visit，则入栈
	            if(out->next_ver->pre == -1)
	            {
	                s.push(out->next_ver);
	                no_out = 0;
	                break;
	            }
	            else
	            {
	                //关键步骤：如果是回边，则返回false
	                if(out->next_ver->post == -1)
	                    return false;
	            }
	        }
	        //如果没有出度，则post
	        if(no_out)
	        {
	            now->post = clock;
	            s.pop();
	            rec++;
	        }
	    }
	    return rec==numCourses;
	}
	
	bool canFinish(int numCourses, vector<pair<int, int> >& prerequisites) {
	    clock = 0;
	
	    struct Vertex * vers = new Vertex[numCourses];
	    for(int i=0;i<numCourses;i++)
	    {
	        vers[i].id = i;
	        vers[i].pre = -1;
	        vers[i].post = -1;
	    }
	
	    //memset(vers,0,sizeof(vers));
	    for(int i=0;i<prerequisites.size();i++)
	    {
	        //建图
	        int first = prerequisites[i].first;
	        int second = prerequisites[i].second;
	        struct Edge * tmp = new Edge;
	        tmp->next_ver = &vers[first];
	        tmp->last_ver = &vers[second];
	        vers[first].id = first;
	        vers[second].id = second;
	        vers[first].in_edges.push_back(tmp);
	        vers[second].out_edges.push_back(tmp);
	    }
	    //DFS
	    return check(numCourses,vers);
	}
};

{% endhighlight %}

#### 总结
在此代码中，我先设各个点的初始 pre 和 post 都为 -1，那么可以知道，在到达一个点后，判断下一条边为什么边时，则可以这么判断：
- 邻接点 pre != -1 && post == -1 -\> 回边
- 邻接点 pre == -1 && post == -1 -\> 树边
- 邻接点 pre != -1 && post != -1 -\> 横跨边或前向边

#### 大神代码

##### DFS

{% highlight C++ linenos %}
class Solution {
public:
	bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
	    vector<unordered_set<int>> graph = make_graph(numCourses, prerequisites);
	    vector<bool> onpath(numCourses, false), visited(numCourses, false);
	    for (int i = 0; i < numCourses; i++)
	        if (!visited[i] && dfs_cycle(graph, i, onpath, visited))
	            return false;
	    return true;
	}
private:
	vector<unordered_set<int>> make_graph(int numCourses, vector<pair<int, int>>& prerequisites) {
	    vector<unordered_set<int>> graph(numCourses);
	    for (auto pre : prerequisites)
	        graph[pre.second].insert(pre.first);
	    return graph;
	} 
	bool dfs_cycle(vector<unordered_set<int>>& graph, int node, vector<bool>& onpath, vector<bool>& visited) {
	    if (visited[node]) return false;
	    onpath[node] = visited[node] = true; 
	    for (int neigh : graph[node])
	        if (onpath[neigh] || dfs_cycle(graph, neigh, onpath, visited))
	            return true;
	    return onpath[node] = false;
	}
};
{% endhighlight %}

##### BFS

{% highlight C++ linenos %}
class Solution {
public:
	bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
	    vector<unordered_set<int>> graph = make_graph(numCourses, prerequisites);
	    vector<int> degrees = compute_indegree(graph);
	    for (int i = 0; i < numCourses; i++) {
	        int j = 0;
	        for (; j < numCourses; j++)
	            if (!degrees[j]) break;
	        if (j == numCourses) return false;
	        degrees[j] = -1;
	        for (int neigh : graph[j])
	            degrees[neigh]--;
	    }
	    return true;
	}
private:
	vector<unordered_set<int>> make_graph(int numCourses, vector<pair<int, int>>& prerequisites) {
	    vector<unordered_set<int>> graph(numCourses);
	    for (auto pre : prerequisites)
	        graph[pre.second].insert(pre.first);
	    return graph;
	}
	vector<int> compute_indegree(vector<unordered_set<int>>& graph) {
	    vector<int> degrees(graph.size(), 0);
	    for (auto neighbors : graph)
	        for (int neigh : neighbors)
	            degrees[neigh]++;
	    return degrees;
	}
}; 
{% endhighlight %}

[1]:	https://leetcode.com/problems/course-schedule/#/description