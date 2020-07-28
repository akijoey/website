---
title: LeetCode - 200
date: 2020-02-21
tags: LeetCode
categories: Algorithm
keywords: leetcode200
description: leetcode200
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/number-of-islands/](https://leetcode.com/problems/number-of-islands/)

![](/img/img_posts/leetcode200.png)

## Write Up

dfs 四个方向遍历岛屿, 将遍历后的岛屿变为海洋.
即将 grid 中的 '1' 变为 '0', 注意边界的判断.
res 保存岛屿数量, 每遍历一个岛屿 res++.

## Source Code

``` c++
/*Number of Islands*/
class Solution
{
	public:
		int direction[4][2] = {{ 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 }};
		void dfs(vector<vector<char>>& grid, int i, int j)
		{
			if (i < 0 || j < 0 || i >= grid.size() || j >= grid[i].size() || grid[i][j] == '0')
				return;
			grid[i][j] = '0';
			for (auto d : direction)
				dfs(grid, i + d[0], j + d[1]);
		}
		int numIslands(vector<vector<char>>& grid)
		{
			int res = 0;
			for (int i = 0;i < grid.size();i++)
				for (int j = 0;j < grid[i].size();j++)
					if (grid[i][j] == '1')
					{
						dfs(grid, i, j);
						res++;
					}
			return res;
		}
};
```
