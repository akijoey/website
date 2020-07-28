---
title: LeetCode - 77
date: 2020-02-24
tags: LeetCode
categories: Algorithm
keywords: leetcode77
description: leetcode77
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/combinations/](https://leetcode.com/problems/combinations/)

![](/img/img_posts/leetcode77.png)

## Write Up

dfs 组合 + 剪枝.
dfs 中 i 的上界其实并不是 n.
而是 n - 待选择个数 + 1, 待选择个数为 k - combination.
即 i <= n - k + combination.size() + 1.

## Source Code

``` c++
/*Combinations*/
class Solution
{
	public:
		vector<vector<int>> res;
		vector<int> combination;
		void dfs(int index, int n, int k)
		{
			if (combination.size() == k)
			{
				res.push_back(combination);
				return;
			}
			for (int i = index;i <= n - k + combination.size() + 1;i++)
			{
				combination.push_back(i);
				dfs(i + 1, n, k);
				combination.pop_back();
			}
		}
		vector<vector<int>> combine(int n, int k)
		{
			dfs(1, n, k);
			return res;
		}
};
```
