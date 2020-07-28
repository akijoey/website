---
title: LeetCode - 645
date: 2019-11-13
tags: LeetCode
categories: Algorithm
keywords: leetcode645
description: leetcode645
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/set-mismatch/](https://leetcode.com/problems/set-mismatch/)

![](/img/img_posts/leetcode645.png)

## Write Up

这道题像是 [LeetCode - 442](https://leetcode.com/problems/find-all-duplicates-in-an-array/) 和 [LeetCode - 448](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/) 的结合体.
丢失了一个元素并且有一个元素重复.
可以利用正负性来判断元素是否存在.
若 nums[abs(nums[i]) - 1] < 0.
则该元素重复出现, 即为重复的元素.
若 nums[i] > 0.
则该元素未出现, 即为丢失的元素.

## Source Code

``` c++
/*Set Mismatch*/
class Solution
{
	public:
		vector<int> findErrorNums(vector<int>& nums)
		{
			vector<int> res;
			for (int i = 0;i < nums.size();i++)
				if (nums[abs(nums[i]) - 1] > 0)
					nums[abs(nums[i]) - 1] *= -1;
				else
					res.push_back(abs(nums[i]));
			for (int i = 0;i < nums.size();i++)
				if (nums[i] > 0)
					res.push_back(i + 1);
			return res;
		}
};
```
