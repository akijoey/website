---
title: LeetCode - 17
date: 2020-02-23
tags: LeetCode
categories: Algorithm
keywords: leetcode17
description: leetcode17
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/letter-combinations-of-a-phone-number/](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

![](/img/img_posts/leetcode17.png)

## Write Up

dfs 全排列.
map[digits[index] - '0'] 获取数字的 map.
因为形参只是 string 的拷贝, 不改变 s 的值.
所以回溯时不需要删除最后一个字符 c.
注意 digits 为 "" 的特殊情况.

## Source Code

``` c++
/*Letter Combinations of a Phone Number*/
class Solution
{
	public:
		vector<string> res;
		string map[10] = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
		void dfs(int index, string s, string& digits)
		{
			if (index == digits.size())
			{
				res.push_back(s);
				return;
			}
			for (auto c : map[digits[index] - '0'])
				dfs(index + 1, s + c, digits);
		}
		vector<string> letterCombinations(string digits)
		{
			if (!digits.size())
				return res;
			dfs(0, "", digits);
			return res;
		}
};
```
