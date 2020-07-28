---
title: LeetCode - 424
date: 2019-10-22
tags: LeetCode
categories: Algorithm
keywords: leetcode424
description: leetcode424
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/longest-repeating-character-replacement/](https://leetcode.com/problems/longest-repeating-character-replacement/)

![](/img/img_posts/leetcode424.png)

## Write Up

滑动窗口算法 (Sliding Window).
用一个 vector 动态维护各个字母出现的次数.
i 为窗口起点, j 为窗口终点.
maxn 记录窗口内出现最多的字母数.
当窗口长度 j - i + 1 大于 maxn + k, 窗口起点向右移动.
窗口长度的最大值即为结果.

## Source Code

``` c++
/*Longest Repeating Character Replacement*/
class Solution
{
	public:
		int characterReplacement(string s, int k)
		{
			vector<int> count(26, 0);
			int res = 0, maxn = 0;
			for (int i = 0, j = 0;j < s.size();j++)
			{
				count[s[j] - 'A']++;
				maxn = max(maxn, count[s[j] - 'A']);
				while (j - i + 1 > maxn + k)
					count[s[i++] - 'A']--;
				res = max(res, j - i + 1);
			}
			return res;
        }
};
```
