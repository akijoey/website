---
title: LeetCode - 526
date: 2020-02-27
tags: LeetCode
categories: Algorithm
keywords: leetcode526
description: leetcode526
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/beautiful-arrangement/](https://leetcode.com/problems/beautiful-arrangement/)

![](/img/img_posts/leetcode526.png)

## Write Up

dfs 全排列 + 剪枝.
vis 数组记录访问状态.
剪去无法整除的情况, 即 !(index % i) || !(i % index).
此处有个非常巧妙的优化, 即 index 从 N 开始递减.
得到的是 beautiful arrangement 的 reverse.
剪枝的过程不同, 得到的数量相同, 即结果相同.
既可以避免除数为 0, 又可以提高剪枝效率.

## Source Code

``` c++
/*Beautiful Arrangement*/
class Solution
{
	public:
		int res = 0;
		void dfs(int index, vector<bool>& vis)
		{
			if (!index)
			{
				res++;
				return;
			}
			for (int i = 1;i <= vis.size();i++)
				if (!vis[i] && (!(index % i) || !(i % index)))
				{
					vis[i] = true;
					dfs(index - 1, vis);
					vis[i] = false;
				}
		}
		int countArrangement(int N)
		{
			vector<bool> vis(N, false);
			dfs(N, vis);
			return res;
		}
};
```
