---
title: LeetCode - 784
date: 2020-02-25
tags: LeetCode
categories: Algorithm
keywords: leetcode784
description: leetcode784
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/letter-case-permutation/](https://leetcode.com/problems/letter-case-permutation/)

![](/img/img_posts/leetcode784.png)

## Write Up

dfs 全排列.
将一次搜索分为字符 c 是否为字母的两种情况.
内置函数 isalpha(c) 判断字符 c 是否为字母.
大写字符与小写字符的 ASCII 差值为 32, 即 1 << 5.
因此只需要将字符 c 异或 1 << 5 即可转换大小写.

## Source Code

``` c++
/*Letter Case Permutation*/
class Solution
{
	public:
		vector<string> res;
		void dfs(int index, string s, string& S)
		{
			if (index == S.size())
			{
				res.push_back(s);
				return;
			}
			char c = S[index];
			dfs(index + 1, s + c, S);
			if (isalpha(c))
			{
				c = S[index] ^ 1 << 5;
				dfs(index + 1, s + c, S);
			}
		}
		vector<string> letterCasePermutation(string S)
		{
			dfs(0, "", S);
			return res;
		}
};
```
