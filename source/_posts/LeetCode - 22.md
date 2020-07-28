---
title: LeetCode - 22
date: 2020-02-23
tags: LeetCode
categories: Algorithm
keywords: leetcode22
description: leetcode22
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/generate-parentheses/](https://leetcode.com/problems/generate-parentheses/)

![](/img/img_posts/leetcode22.png)

## Write Up

dfs + 回溯.
l, r 分别记录左右括号的数量.
若 l < n 则添加左括号, 即 s + '('.
若 l > r 则添加右括号, 即 s + ')'.

## Source Code

``` c++
/*Generate Parentheses*/
class Solution
{
	public:
		vector<string> res;
		void dfs(int l, int r, string s, int n)
		{
			if (l == n && r == n)
				res.push_back(s);
			if (l < n)
				dfs(l + 1, r, s + '(', n);
			if (l > r)
				dfs(l, r + 1, s + ')', n);
		}
		vector<string> generateParenthesis(int n)
		{
			dfs(0, 0, "", n);
			return res;
		}
};
```
