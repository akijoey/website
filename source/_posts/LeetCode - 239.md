---
title: LeetCode - 239
date: 2020-01-15
tags: LeetCode
categories: Algorithm
keywords: leetcode239
description: leetcode239
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/sliding-window-maximum/](https://leetcode.com/problems/sliding-window-maximum/)

![](/img/img_posts/leetcode239.png)

## Write Up

线性时间复杂度需要一个双端队列.
队列保存下标, 每次窗口移动保证队头的值最大.
若窗口右端大于队尾的值, 则删除队尾.
直到窗口右端小于队尾的值或队列为空, 窗口右端下标入队.
若队头的值不在窗口内, 则删除队头.
队头即为窗口最大值.

## Source Code

``` c++
/*Sliding Window Maximum*/
class Solution
{
	public:
		vector<int> maxSlidingWindow(vector<int>& nums, int k)
		{
			vector<int> res;
			deque<int> que;
			for (int i = 0;i < nums.size();i++)
			{
				while (!que.empty() && nums[i] >= nums[que.back()])
					que.pop_back();
				que.push_back(i);
				if (que.front() <= i - k)
						que.pop_front();
				if (i + 1 >= k)
					res.push_back(nums[que.front()]);
			}
			return res;
		}
};
```
