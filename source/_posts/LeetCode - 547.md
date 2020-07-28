---
title: LeetCode - 547
date: 2020-02-22
tags: LeetCode
categories: Algorithm
keywords: leetcode547
description: leetcode547
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/friend-circles/](https://leetcode.com/problems/friend-circles/)

![](/img/img_posts/leetcode547.png)

## Write Up

并查集 + 路径压缩优化.
先假设共有 n 个朋友圈.
若两个朋友圈能够合并, 则 res--.

## Source Code

``` c++
/*Friend Circles*/
class Solution
{
	public:
		vector<int> parent;
		int find(int v)
		{
			return parent[v] = parent[v] == v ? v : find(parent[v]);
		}
		bool unite(int x, int y)
		{
			int a = find(x), b = find(y);
			if (a != b)
			{
				parent[a] = b;
				return true;
			}
			return false;
		}
		int findCircleNum(vector<vector<int>>& M)
		{
			int n = M.size(), res = n;
			for (int i = 0;i < n;i++)
				parent.push_back(i);
			for (int i = 0;i < n;i++)
				for (int j = i + 1;j < n;j++)
					if (M[i][j] && unite(i, j))
						res--;
			return res;
		}
};
```
