---
title: LeetCode - 216
date: 2020-02-27
tags: LeetCode
categories: Algorithm
keywords: leetcode216
description: leetcode216
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/combination-sum-iii/](https://leetcode.com/problems/combination-sum-iii/)

![](/img/img_posts/leetcode216.png)

## Write Up

dfs 组合 + 剪枝.
剪去组合总和大于 n 的情况.
n 逐层减去 i, 因此只需要剪去 n < i 的情况.

## Source Code

``` c++
/*Combination Sum III*/
class Solution
{
	public:
		vector<vector<int>> res;
		vector<int> candidate;
		void dfs(int index, int k, int n)
		{
			if (!n && candidate.size() == k)
			{
				res.push_back(candidate);
				return;
			}
			for (int i = index;i < 10;i++)
				if (n >= i)
				{
					candidate.push_back(i);
					dfs(i + 1, k, n - i);
					candidate.pop_back();
				}
		}
		vector<vector<int>> combinationSum3(int k, int n)
		{
			dfs(1, k, n);
			return res;
		}
};
```
