---
title: LeetCode - 78
date: 2020-02-24
tags: LeetCode
categories: Algorithm
keywords: leetcode78
description: leetcode78
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/subsets/](https://leetcode.com/problems/subsets/)

![](/img/img_posts/leetcode78.png)

## Write Up

dfs 组合.
将一次搜索分为是否选取某元素的两种情况.
分别对应 push 和 pop 后的 dfs.

## Source Code

``` c++
/*Subsets*/
class Solution
{
	public:
		vector<vector<int>> res;
		vector<int> subset;
		void dfs(int index, vector<int>& nums)
		{
			if (index == nums.size())
			{
				res.push_back(subset);
				return;
			}
			subset.push_back(nums[index]);
			dfs(index + 1, nums);
			subset.pop_back();
			dfs(index + 1, nums);
		}
		vector<vector<int>> subsets(vector<int>& nums)
		{
			dfs(0, nums);
			return res;
		}
};
```
