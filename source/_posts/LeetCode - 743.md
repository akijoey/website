---
title: LeetCode - 743
date: 2020-01-12
tags: LeetCode
categories: Algorithm
keywords: leetcode743
description: leetcode743
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/network-delay-time/](https://leetcode.com/problems/network-delay-time/)

![](/img/img_posts/leetcode743.png)

## Write Up

简单的单源最短路问题.
就用简单的 Bellman - Ford 算法.
对所有边进行松弛操作, 重复 n - 1 次.
若不存在路径变化, 则已得到最短路径.

## Source Code

``` c++
/*Network Delay Time*/
#define INF 0x3f3f3f3f
class Solution
{
	public:
		int networkDelayTime(vector<vector<int>>& times, int N, int K)
		{
			int res = 0;
			bool loop = true;
			vector<int> dis(N + 1, INF);
			dis[K] = 0;
			for (int i = 1;i < N;i++)
			{
				loop = false;
				for (int j = 0;j < times.size();j++)
					if (dis[times[j][0]] != INF)
						if (dis[times[j][0]] + times[j][2] < dis[times[j][1]])
						{
							dis[times[j][1]] = dis[times[j][0]] + times[j][2];
							loop = true;
						}
				if (!loop)
					break;
			}
			for (int i = 1;i <= N;i++)
				if (dis[i] == INF)
					return -1;
				else if (dis[i] > res)
					res = dis[i];
			return res;
		}
};
```
