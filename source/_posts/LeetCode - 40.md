---
title: LeetCode - 40
date: 2020-02-23
tags: LeetCode
categories: Algorithm
keywords: leetcode40
description: leetcode40
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/combination-sum-ii/](https://leetcode.com/problems/combination-sum-ii/)

![](/img/img_posts/leetcode40.png)

## Write Up

dfs 组合 + 剪枝.
在上一题的基础上添加一个剪枝.
首先对 candidates 排序, 使相同数字处于相邻位置.
剪去后一个位置即 candidates[i] == candidates[i - 1] 的情况.

## Source Code

``` c++
/*Combination Sum II*/
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
			{
				if (target < candidates[i])
					break;
				if (i > index && candidates[i] == candidates[i - 1])
					continue;
				candidate.push_back(candidates[i]);
				dfs(i + 1, target - candidates[i], candidates);
				candidate.pop_back();
			}
		}
		vector<vector<int>> combinationSum2(vector<int>& candidates, int target)
		{
			sort(candidates.begin(), candidates.end());
			dfs(0, target, candidates);
			return res;
		}
};
```
