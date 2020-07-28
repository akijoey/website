---
title: LeetCode - 1254
date: 2020-02-27
tags: LeetCode
categories: Algorithm
keywords: leetcode1254
description: leetcode1254
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/number-of-closed-islands/](https://leetcode.com/problems/number-of-closed-islands/)

![](/img/img_posts/leetcode1254.png)

## Write Up

dfs 四个方向遍历岛屿, 将遍历后的岛屿变为海洋.
即将 grid 中的 1 变为 0, 同时判断该岛屿是否封闭.
若岛屿碰上边界, 则该岛屿不封闭, 即 num 为 0.

## Source Code

``` c++
/*Number of Closed Islands*/
class Solution
{
	public:
		int direction[4][2] = {{ 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 }};
		int dfs(vector<vector<int>>& grid, int i, int j)
		{
			if (i < 0 || j < 0 || i >= grid.size() || j >= grid[i].size())
				return 0;
			if (grid[i][j])
				return 1;
			grid[i][j] = 1;
			int num = 1;
			for (auto d : direction)
				num *= dfs(grid, i + d[0], j + d[1]);
			return num;
		}
		int closedIsland(vector<vector<int>>& grid)
		{
			int res = 0;
			for (int i = 0;i < grid.size();i++)
				for (int j = 0;j < grid[i].size();j++)
					if (grid[i][j] == 0)
						res += dfs(grid, i, j);
			return res;
		}
};
```
