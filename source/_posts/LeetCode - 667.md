---
title: LeetCode - 667
date: 2020-02-27
tags: LeetCode
categories: Algorithm
keywords: leetcode667
description: leetcode667
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/beautiful-arrangement-ii/](https://leetcode.com/problems/beautiful-arrangement-ii/)

![](/img/img_posts/leetcode667.png)

## Write Up

第一个方法是 dfs + 剪枝, 超时了.
这道题其实可以直接构造出结果.
1 到 n 个不同整数的差值个数最多为 n - 1.
即 k < n, 因此只需要在 n 个数的排列里构造 k 个差值, 即 1 到 k.
较大的数和较小的数交替出现 k 次, 即差值为 2 到 k.
剩余 n - k 个数有序排列, 即差值都为 1.

## Source Code

``` c++
/*Beautiful Arrangement II*/
class Solution
{
	public:
		vector<int> constructArray(int n, int k)
		{
			vector<int> res;
			for (int i = 1, j = n;i <= j;)
				if (k > 1)
					res.push_back(k-- % 2 ? i++ : j--);
				else
					res.push_back(i++);
			return res;
		}
};
```
