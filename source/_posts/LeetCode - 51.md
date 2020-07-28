---
title: LeetCode - 51
date: 2020-02-26
tags: LeetCode
categories: Algorithm
keywords: leetcode51
description: leetcode51
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/n-queens/](https://leetcode.com/problems/n-queens/)

![](/img/img_posts/leetcode51.png)

## Write Up

dfs + 回溯 + 剪枝.
搜索每个 grid 做合法性检验.
检验其所在列以及主副对角线上是否存在皇后.

## Source Code

``` c++
/*N-Queens*/
class Solution
{
	public:
		vector<vector<string>> res;
		bool check(vector<string>& grid, int row, int col)
		{
			for (int i = 0;i < row;i++)
				if (grid[i][col] == 'Q')
					return false;
			for (int i = row - 1, j = col - 1;i >=0 && j >= 0;i--, j--)
				if (grid[i][j] == 'Q')
					return false;
			for (int i = row - 1, j = col + 1;i >= 0 && j < grid.size();i--, j++)
				if (grid[i][j] == 'Q')
					return false;
			return true;
		}
		void dfs(int index, vector<string>& grid)
		{
			if (index == grid.size())
			{
				res.push_back(grid);
				return;
			}
			for (int i = 0;i < grid.size();i++)
				if (check(grid, index, i))
				{
					grid[index][i] = 'Q';
					dfs(index + 1, grid);
					grid[index][i] = '.';
				}
		}
		vector<vector<string>> solveNQueens(int n)
		{
			vector<string> grid(n, string(n, '.'));
			dfs(0, grid);
			return res;
		}
};
```
