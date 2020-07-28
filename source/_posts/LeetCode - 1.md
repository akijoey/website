---
title: LeetCode - 1
date: 2019-10-22
tags: LeetCode
categories: Algorithm
keywords: leetcode1
description: leetcode1
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)

![](/img/img_posts/leetcode1.png)

## Write Up

hash table 存储查找.
遍历 nums, 得到差值 target - nums[i].
用 map 容器存储 nums, 即 nums 的值作为键, 下标作为值.
若 map 中找到差值, 即为结果.

## Source Code

``` c++
/*Two Sum*/
class Solution
{
	public:
		vector<int> twoSum(vector<int>& nums, int target)
		{
			map<int, int> map;
			vector<int> res;
			for (int i = 0;i < nums.size();i++)
			{
				if (map.find(target - nums[i]) != map.end())
				{
					res.push_back(map[target - nums[i]]);
					res.push_back(i);
					return res;
				}
				else
					map[nums[i]] = i;
			}
			return res;
        }
};
```
