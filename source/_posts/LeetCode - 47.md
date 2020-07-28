---
title: LeetCode - 47
date: 2020-02-22
tags: LeetCode
categories: Algorithm
keywords: leetcode47
description: leetcode47
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/permutations-ii/](https://leetcode.com/problems/permutations-ii/)

![](/img/img_posts/leetcode47.png)

## Write Up

dfs 全排列 + 剪枝.
首先对 nums 排序, 确保 nums 有序.
剪去相同数字交换即 nums[i] == nums[index] 的情况.
另一个方法是用 set<vector<int>> 保存结果去重.

## Source Code

``` c++
/*Permutations II*/
class Solution
{
	public:
		vector<vector<int>> res;
		void dfs(int index, vector<int> nums)
		{
			if (index == nums.size())
				res.push_back(nums);
			for (int i = index;i < nums.size();i++)
			{
				if (i > index && nums[i] == nums[index])
					continue;
				swap(nums[i], nums[index]);
				dfs(index + 1, nums);
			}
		}
		vector<vector<int>> permuteUnique(vector<int>& nums)
		{
			sort(nums.begin(), nums.end());
			dfs(0, nums);
			return res;
		}
};
```
