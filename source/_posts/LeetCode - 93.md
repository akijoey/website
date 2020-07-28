---
title: LeetCode - 93
date: 2020-02-25
tags: LeetCode
categories: Algorithm
keywords: leetcode93
description: leetcode93
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/restore-ip-addresses/](https://leetcode.com/problems/restore-ip-addresses/)

![](/img/img_posts/leetcode93.png)

## Write Up

dfs + 回溯 + 剪枝.
ipv4 地址分为四段, 即搜索树为 4 层.
每段长度为 1 到 3, 即 i 从 1 到 3.
剪枝条件为每段数值小于 255 且不以 0 开头.
stoi() 将字符串转为整数, to_string() 将整数转为字符串.
若 i != to_string(paragraph).size() 则说明开头存在 0.

## Source Code

``` c++
/*Restore IP Addresses*/
class Solution
{
	public:
		vector<string> res;
		void dfs(int index, string ip, string s)
		{
			if (index == 4)
			{
				if (s.empty())
					res.push_back(ip);
				return;
			}
			for (int i = 1;i < 4;i++)
			{
				if (s.size() < i)
					break;
				int paragraph = stoi(s.substr(0, i));
				if (paragraph > 255 || i != to_string(paragraph).size())
					continue;
				dfs(index + 1, ip + s.substr(0, i) + (index == 3 ? "" : "."), s.substr(i));
			}
		}
		vector<string> restoreIpAddresses(string s)
		{
			dfs(0, "", s);
			return res;
		}
};
```
