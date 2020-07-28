---
title: LeetCode - 90
date: 2020-02-24
tags: LeetCode
categories: Algorithm
keywords: leetcode90
description: leetcode90
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/subsets-ii/](https://leetcode.com/problems/subsets-ii/)

![](/img/img_posts/leetcode90.png)

## Write Up

dfs 组合 + 剪枝.
在 78 题的基础上添加一个剪枝.
首先对 nums 排序, 使相同数字处于相邻位置.
确保 index + 1 < nums.size() 防止下标越界.
剪去 nums[index] == nums[index + 1] 时的右侧分支.

## Source Code

``` c++
/*Subsets II*/
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
			while (index + 1 < nums.size() && nums[index] == nums[index + 1])
				index++;
			dfs(index + 1, nums);
		}
		vector<vector<int>> subsetsWithDup(vector<int>& nums)
		{
			sort(nums.begin(), nums.end());
			dfs(0, nums);
			return res;
		}
};
```
