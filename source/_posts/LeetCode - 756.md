---
title: LeetCode - 756
date: 2020-03-14
tags: LeetCode
categories: Algorithm
keywords: leetcode756
description: leetcode756
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/pyramid-transition-matrix/](https://leetcode.com/problems/pyramid-transition-matrix/)

![](/img/img_posts/leetcode756.png)

## Write Up

dfs 逐层堆砌金字塔.
用一个 map 保存堆砌的映射规则.
current 表示当前层级的字符串, bottom 表示下层的字符串.
index == bottom.size() - 1 表示当前层级堆砌完成, 堆砌上一层.

## Source Code

``` c++
/*Pyramid Transition Matrix*/
class Solution
{
	public:
		unordered_map<string, vector<char>> map;
		bool dfs(int index, string current, string bottom)
		{
			if (bottom.size() == 1)
				return true;
			if (index == bottom.size() - 1)
				return dfs(0, "", current);
			for (char c : map[bottom.substr(index, 2)])
				if (dfs(index + 1, current + c, bottom))
					return true;
			return false;
		}
		bool pyramidTransition(string bottom, vector<string>& allowed)
		{
			for(string& s: allowed)
				map[s.substr(0, 2)].push_back(s[2]);
			return dfs(0, "", bottom);
		}
};
```
