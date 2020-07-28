---
title: LeetCode - 713
date: 2020-01-11
tags: LeetCode
categories: Algorithm
keywords: leetcode713
description: leetcode713
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/subarray-product-less-than-k/](https://leetcode.com/problems/subarray-product-less-than-k/)

![](/img/img_posts/leetcode713.png)

## Write Up

滑动窗口算法 (Sliding Window).
i 为窗口起点, j 为窗口终点, pro 记录窗口乘积.
当窗口乘积 pro 大于等于 k, 窗口起点向右移动.
窗口长度即为子数组个数, 子数组个数之和即为结果.
注意处理两个边界情况 k = 0 和 k = 1.

## Source Code

``` c++
/*Subarray Product Less Than K*/
class Solution
{
	public:
		int numSubarrayProductLessThanK(vector<int>& nums, int k)
		{
			if (k <= 1)
				return 0;
			int res = 0, pro = 1;
			for (int i = 0, j = 0;j < nums.size();j++)
			{
				pro *= nums[j];
				while (pro >= k)
					pro /= nums[i++];
				res += j - i + 1;
			}
			return res;
        }
};
```
