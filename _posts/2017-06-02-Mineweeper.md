---
layout: post
title: 529. Minesweeper
tags: [LeetCode]
---

*source:[LeetCode][1]*


{% highlight C++ linenos %}
#include<vector>
using namespace std;
class Solution {
public:
    vector<vector<char> > updateBoard(vector<vector<char> >& board, vector<int>& click) {
        
        if (board[click[0]][click[1]] == 'M') 
            board[click[0]][click[1]] = 'X';
        else
        {
            wrap(board);
            int now[] = {click[0]+1, click[1]+1};
            dfs(board, now);
            unwrap(board);
        }
       
        
        return board;
    }

    void wrap(vector<vector<char> >& board)
    {
        vector<char> cols;
        for (int i=0; i<board[0].size(); i++)
            cols.push_back('B');
        board.push_back(cols);
        board.insert(board.begin(), cols);
        for (int i=0; i<board.size(); i++)
        {
            board[i].push_back('B');
            board[i].insert(board[i].begin(), 'B');
        }
    }

    void unwrap(vector<vector<char> >& board)
    {
        board.erase(board.begin());
        board.erase(board.end());
        
        for(int i=0; i<board.size(); i++)
        {
            board[i].erase(board[i].begin());
           
            board[i].pop_back();
        }
    }

    void dfs(vector<vector<char> >& board, int now[])
    {
        
        if(now[0] == 0 || now[1] == 0 || now[0] == board.size()-1 || now[1] == board[0].size())return;
       
        char now_stat = board[now[0]][now[1]];
        
        int up[2] = {now[0]-1, now[1]};
        int down[2] = {now[0]+1, now[1]};
        int left[2] = {now[0], now[1]-1};
        int right[2] = {now[0], now[1]+1};
        int l_u[2] = {now[0]-1, now[1]-1};
        int l_d[2] = {now[0]+1, now[1]-1};
        int r_u[2] = {now[0]-1, now[1]+1};
        int r_d[2] = {now[0]+1, now[1]+1};

        int mine_num = 0;

        if (now_stat == 'E')
        {
            mine_num = 0;
            // has mine adjent
            if (board[up[0]][up[1]] == 'M') mine_num++;
            
            if (board[down[0]][down[1]] == 'M') mine_num++;
            if (board[left[0]][left[1]] == 'M') mine_num++;
            if (board[right[0]][right[1]] == 'M') mine_num++;
            
            if (board[l_d[0]][l_d[1]] == 'M') mine_num++;
            if (board[l_u[0]][l_u[1]] == 'M') mine_num++;
            if (board[r_d[0]][r_d[1]] == 'M') mine_num++;
         
            if (board[r_u[0]][r_u[1]] == 'M') mine_num++;
            
            if(mine_num == 0)
            {
                
                board[now[0]][now[1]] = 'B';
                 
                dfs(board, up);
                dfs(board, down);
                dfs(board, left);
                dfs(board, right);
                dfs(board, l_d);
                dfs(board, l_u);
                dfs(board, r_d);
                dfs(board, r_u);
                
            }
            else
                board[now[0]][now[1]] = mine_num+48;
            
        }
         
    }
};

/* summary
 * 1. 为了防止 DFS 越界问题，请将下标作为参数传递。
*/

{% endhighlight %}


[1]:	https://leetcode.com/problems/minesweeper/#/description
