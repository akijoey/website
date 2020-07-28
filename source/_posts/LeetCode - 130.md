---
title: LeetCode - 130
date: 2020-03-12
tags: LeetCode
categories: Algorithm
keywords: leetcode130
description: leetcode130
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/surrounded-regions/](https://leetcode.com/problems/surrounded-regions/)

![](/img/img_posts/leetcode130.png)

## Write Up

dfs 四个方向遍历矩阵, 将被 'X' 包围的 'O' 变成 'X'.
与 [LeetCode - 1254](https://leetcode.com/problems/number-of-closed-islands/) 求封闭岛屿的个数类似.
但是这道题不是求个数, 只能将封闭区域的 'O' 变成 'X'.
先将未封闭的区域标记出来, 然后遍历矩阵更新状态.
只需要搜索边缘, 即 !i || !j || i == board.size() - 1 || j == board[0].size() - 1.
同时引入一个临时状态 '#' 表示边缘的某个点已经搜索过.
此时未封闭区域为 '#', 封闭区域为 'O'.
遍历矩阵更新状态, 将 'O' 改为 'X', '#' 改为 'O'.

## Source Code

``` c++
/*Surrounded Regions*/
class Solution
{
	public:
		int direction[4][2] = {{ 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 }};
		void dfs(vector<vector<char>>& board, int i, int j)
		{
			if (i < 0 || j < 0 || i >= board.size() || j >= board[i].size() || board[i][j] == 'X' || board[i][j] == '#')
				return;
			board[i][j] = '#';
			for (auto d : direction)
				dfs(board, i + d[0], j + d[1]);
		}
		void solve(vector<vector<char>>& board)
		{
			for (int i = 0;i < board.size();i++)
				for (int j = 0;j < board[0].size();j++)
					if (board[i][j] == 'O' && (!i || !j || i == board.size() - 1 || j == board[0].size() - 1))
						dfs(board, i, j);
			for (int i = 0;i < board.size();i++)
				for (int j = 0;j < board[0].size();j++)
					if (board[i][j] == 'O')
						board[i][j] = 'X';
					else if (board[i][j] == '#')
						board[i][j] = 'O';
		}
};
```
