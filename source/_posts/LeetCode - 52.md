---
title: LeetCode - 52
date: 2020-02-26
tags: LeetCode
categories: Algorithm
keywords: leetcode52
description: leetcode52
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/n-queens-ii/](https://leetcode.com/problems/n-queens-ii/)

![](/img/img_posts/leetcode52.png)

## Write Up

dfs + 位压缩.
思路与上一题一样, 通过位压缩优化了剪枝条件.
col, hill, dale 分别表示每一列, 主副对角线的状态.
状态为是否存在皇后, 即状态只能为 0 或 1.
因此可以通过位压缩降低空间复杂度.

## Source Code

``` c++
/*N-Queens II*/
class Solution
{
	public:
		int res = 0;
		void dfs(int index, int col, int hill, int dale, int n)
		{
			if (index == n)
			{
				res++;
				return;
			}
			for (int i = 0;i < n;i++)
				if (!(col >> i & 1) && !(hill >> index + i & 1) && !(dale >> index - i + n & 1))
					dfs(index + 1, col | 1 << i, hill | 1 << index + i, dale | 1 << index - i + n, n);
		}
		int totalNQueens(int n)
		{
			dfs(0, 0, 0, 0, n);
			return res;
		}
};
```
