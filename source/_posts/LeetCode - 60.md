---
title: LeetCode - 60
date: 2020-02-25
tags: LeetCode
categories: Algorithm
keywords: leetcode60
description: leetcode60
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/permutation-sequence/](https://leetcode.com/problems/permutation-sequence/)

![](/img/img_posts/leetcode60.png)

## Write Up

直接找出第 k 个排列.
n 的范围为 1 到 9, 可对阶乘打表.
首先 k-- 将 k 转换为下标形式.
若确定了第一个数, 则剩下 (n - 1)! 种排列.
即 k / factorial[n - 1] 可确定第一个数.
更新 k 的值, nums 中删除已经确定的数.
迭代 n 次即可得到结果.

## Source Code

``` c++
/*Permutation Sequence*/
class Solution
{
	public:
		string getPermutation(int n, int k)
		{
			string res;
			string nums = "123456789";
			int factorial[] = { 1, 1, 2, 6, 24, 120, 720, 5040, 40320, 362880, 3628800 };
			k--;
			while (n)
			{
				int i = k / factorial[n - 1];
				k %= factorial[n-- - 1];
				res.push_back(nums[i]);
				nums.erase(i, 1);
			}
			return res;
		}
};
```
