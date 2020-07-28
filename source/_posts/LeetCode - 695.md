---
title: LeetCode - 695
date: 2020-02-21
tags: LeetCode
categories: Algorithm
keywords: leetcode695
description: leetcode695
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/max-area-of-island/](https://leetcode.com/problems/max-area-of-island/)

![](/img/img_posts/leetcode695.png)

## Write Up

dfs 四个方向遍历岛屿, 将遍历后的岛屿变为海洋.
即将 grid 中的 1 变为 0, 注意边界的判断.
area 计算岛屿面积, res 为岛屿面积最大值.

## Source Code

``` c++
/*Max Area of Island*/
class Solution
{
	public:
		int direction[4][2] = {{ 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 }};
		int dfs(vector<vector<int>>& grid, int i, int j)
		{
			if (i < 0 || j < 0 || i >= grid.size() || j >= grid[i].size() || !grid[i][j])
				return 0;
			grid[i][j] = 0;
			int area = 1;
			for (auto d : direction)
				area += dfs(grid, i + d[0], j + d[1]);
			return area;
		}
		int maxAreaOfIsland(vector<vector<int>>& grid)
		{
			int res = 0;
			for (int i = 0;i < grid.size();i++)
				for (int j = 0;j < grid[i].size();j++)
					if (grid[i][j])
						res = max(res, dfs(grid, i, j));
			return res;
		}
};
```
