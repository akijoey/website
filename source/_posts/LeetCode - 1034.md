---
title: LeetCode - 1034
date: 2020-03-13
tags: LeetCode
categories: Algorithm
keywords: leetcode1034
description: leetcode1034
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/coloring-a-border/](https://leetcode.com/problems/coloring-a-border/)

![](/img/img_posts/leetcode1034.png)

## Write Up

dfs 四个方向遍历矩阵, 将指定连通分量的边界染成指定颜色.
先将连通分量的所有网格的颜色取反, 用于表示访问状态.
若某网格与四周同色, 则该网格不为连通分量的边界.
因此只需要在回溯时将四周同色的网格的颜色还原.
除去四周同色的网格, 剩余标记过的网格即为连通分量的边界.
遍历矩阵更新状态, 将标记过的网格进行着色.

## Source Code

``` c++
/*Coloring A Border*/
class Solution
{
	public:
		int direction[4][2] = {{ 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 }};
		void dfs(vector<vector<int>>& grid, int i, int j, int color)
		{
			if (i < 0 || j < 0 || i >= grid.size() || j >= grid[0].size() || grid[i][j] != color)
				return;
			grid[i][j] *= -1;
			for (auto d : direction)
				dfs(grid, i + d[0], j + d[1], color);
			if (i > 0 && i < grid.size() - 1 && j > 0 && j < grid[0].size() - 1)
				if (color == abs(grid[i - 1][j]) && color == abs(grid[i + 1][j]) && color == abs(grid[i][j - 1]) && color == abs(grid[i][j + 1]))
					grid[i][j] *= -1;
		}
		vector<vector<int>> colorBorder(vector<vector<int>>& grid, int r0, int c0, int color)
		{
			dfs(grid, r0, c0, grid[r0][c0]);
			for (int i = 0;i < grid.size();i++)
				for (int j = 0;j < grid[0].size();j++)
					grid[i][j] = grid[i][j] < 0 ? color : grid[i][j];
			return grid;
		}
};
```
