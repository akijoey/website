---
title: LeetCode - 39
date: 2020-02-23
tags: LeetCode
categories: Algorithm
keywords: leetcode39
description: leetcode39
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/combination-sum/](https://leetcode.com/problems/combination-sum/)

![](/img/img_posts/leetcode39.png)

## Write Up

dfs 组合 + 剪枝.
剪去组合总和大于 target 的情况.
因为 target 逐层减去 candidates[i].
所以只需要剪去 target < candidates[i] 的情况.
因为数字可以重复使用, 所以起始位置还是 index 自身.

## Source Code

``` c++
/*Combination Sum*/
class Solution
{
	public:
		vector<vector<int>> res;
		vector<int> candidate;
		void dfs(int index, int target, vector<int>& candidates)
		{
			if (!target)
			{
				res.push_back(candidate);
				return;
			}
			for (int i = index;i < candidates.size();i++)
				if (target >= candidates[i])
				{
					candidate.push_back(candidates[i]);
					dfs(i, target - candidates[i], candidates);
					candidate.pop_back();
				}
		}
		vector<vector<int>> combinationSum(vector<int>& candidates, int target)
		{
			dfs(0, target, candidates);
			return res;
		}
};
```
