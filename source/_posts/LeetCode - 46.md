---
title: LeetCode - 46
date: 2020-02-22
tags: LeetCode
categories: Algorithm
keywords: leetcode46
description: leetcode46
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/permutations/](https://leetcode.com/problems/permutations/)

![](/img/img_posts/leetcode46.png)

## Write Up

dfs 全排列.
因为形参不是 nums 数组的引用.
所以回溯时不需要 swap.
另一个方法是使用内置函数 next_permutation().

## Source Code

``` c++
/*Permutations*/
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
				swap(nums[i], nums[index]);
				dfs(index + 1, nums);
			}
		}
		vector<vector<int>> permute(vector<int>& nums)
		{
			dfs(0, nums);
			return res;
		}
};
```
