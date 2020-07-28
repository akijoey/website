---
title: LeetCode - 442
date: 2019-11-13
tags: LeetCode
categories: Algorithm
keywords: leetcode442
description: leetcode442
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/find-all-duplicates-in-an-array/](https://leetcode.com/problems/find-all-duplicates-in-an-array/)

![](/img/img_posts/leetcode442.png)

## Write Up

不使用额外空间, 就只能在原数组上操作.
1 ≤ a[i] ≤ n, 因此元素之间存在映射.
可以利用正负性来判断元素是否存在.
若 nums[abs(nums[i]) - 1] < 0.
则该元素重复出现, 即为结果.

## Source Code

``` c++
/*Find All Duplicates in an Array*/
class Solution
{
	public:
		vector<int> findDuplicates(vector<int>& nums)
		{
			vector<int> res;
			for (int i = 0;i < nums.size();i++)
				if (nums[abs(nums[i]) - 1] > 0)
					nums[abs(nums[i]) - 1] *= -1;
				else
					res.push_back(abs(nums[i]));
			return res;
		}
};
```
