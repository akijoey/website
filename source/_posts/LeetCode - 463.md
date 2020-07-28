---
title: LeetCode - 463
date: 2020-02-21
tags: LeetCode
categories: Algorithm
keywords: leetcode463
description: leetcode463
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/island-perimeter/](https://leetcode.com/problems/island-perimeter/)

![](/img/img_posts/leetcode463.png)

## Write Up

因为恰好只有一个岛屿, 不需要 dfs.
某网格为陆地, 即 grid[i][j] == 1, 则周长 res + 4,
若该网格左方 grid[i - 1][j] == 1, 则重复了两条边, 即 res - 2.
若该网格上方 grid[i][j - 1] == 1, 则重复了两条边, 即 res - 2.

## Source Code

``` c++
/*Island Perimeter*/
class Solution
{
	public:
		int islandPerimeter(vector<vector<int>>& grid)
		{
			int res = 0;
			for (int i = 0;i < grid.size();i++)
				for (int j = 0;j < grid[i].size();j++)
					if (grid[i][j])
					{
						res += 4;
						if (i && grid[i - 1][j])
							res -= 2;
						if (j && grid[i][j - 1])
							res -= 2;
					}
			return res;
		}
};
```
