---
title: LeetCode - 719
date: 2019-11-06
tags: LeetCode
categories: Algorithm
keywords: leetcode719
description: leetcode719
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/find-k-th-smallest-pair-distance/](https://leetcode.com/problems/find-k-th-smallest-pair-distance/)

![](/img/img_posts/leetcode719.png)

## Write Up

设数组元素有 n 个, 则有 n * (n - 1) / 2个差值.
将数组排序, 差值的取值范围为 [0, nums.back() - nums[0]].
寻找第 k 个最小差值, 可以进行二分查找.
count(nums, mid) 计算差值小于等于 mid 的个数.
利用滑动窗口算法, i 为窗口起点, j 为窗口终点.
当窗口两端差值 nums[j] - nums[i] < mid 时.
窗口长度即为结果个数, 即 res += j - i.
若差值个数大于等于 k, 则 mid 比目标值大, r = mid 向左边查找.
反之则 mid 比目标值小, l = mid + 1 向右边查找.
最终查找结果即为第 k 个最小差值.

## Source Code

``` c++
/*Find K-th Smallest Pair Distance*/
class Solution
{
	public:
		int count(vector<int>& nums, int mid)
		{
			int res = 0;
			for (int i = 0, j = 0;j < nums.size();j++)
			{
				while (nums[j] - nums[i] > mid)
					i++;
				res += j - i;
			}
			return res;
		}
		int smallestDistancePair(vector<int>& nums, int k)
		{
			sort(nums.begin(), nums.end());
			int l = 0, r = nums.back() - nums[0];
			while (l < r)
			{
				int mid = (l + r) / 2;
				if (count(nums, mid) >= k)
					r = mid;
				else
					l = mid + 1;
			}
			return l;
		}
};
```
