---
title: LeetCode - 1376
date: 2020-03-15
tags: LeetCode
categories: Algorithm
keywords: leetcode1376
description: leetcode1376
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/time-needed-to-inform-all-employees/](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

![](/img/img_posts/leetcode1376.png)

## Write Up

记忆化搜索.
time[i] 保存 id 为 i 的员工收到通知的时间.
搜索所有未收到通知的员工, 取所需时间的最大值即可.

## Source Code

``` c++
/*Time Needed to Inform All Employees*/
class Solution
{
	public:
		int dfs(int index, int headID, vector<int>& manager, vector<int>& informTime, vector<int>& time)
		{
			if (index == headID)
				return 0;
			if (time[index] > 0)
				return time[index];
			time[index] = dfs(manager[index], headID, manager, informTime, time) + informTime[manager[i]];
			return time[index];
		}
		int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime)
		{
			int res = 0;
			vector<int> time(n, 0);
			for (int i = 0;i < n;i++)
				res = max(res, dfs(i, headID, manager, informTime, time));
			return res;
		}
};
```
