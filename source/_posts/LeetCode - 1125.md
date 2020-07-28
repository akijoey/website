---
title: LeetCode - 1125
date: 2019-11-07
tags: LeetCode
categories: Algorithm
keywords: leetcode1125
description: leetcode1125
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/smallest-sufficient-team/](https://leetcode.com/problems/smallest-sufficient-team/)

![](/img/img_posts/leetcode1125.png)

## Write Up

DFS + 位压缩.
map 存储 skill 信息, man 数组实现位压缩.
将每个 skill 压缩为 1 << map[people[i][j]].
man 数组即为压缩后的 people 数组.
DFS 中的 skill 为当前 team 中需要的 skill.
若 man 中有人掌握此 skill, 则加入 team.
res 数组保存人数最小的 team, 即为结果.

## Source Code

``` c++
/*Smallest Sufficient Team*/
class Solution
{
	public:
		void dfs(int index, int len, vector<int>& man, vector<int>& team, vector<int>& res)
		{
			if (index == (1 << len) - 1)
			{
				if (!res.size() || team.size() < res.size())
					res = team;
				return;
			}
			if (res.size() && team.size() >= res.size())
				return;
			int skill = 0;
			while (((index >> skill) & 1) == 1)
				skill++;
			for (int i = 0;i < man.size();i++)
				if (((man[i] >> skill) & 1) == 1)
				{
					team.push_back(i);
					dfs(index | man[i], len, man, team, res);
					team.pop_back();
				}
		}
		vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people)
		{
			unordered_map<string, int> map;
			vector<int> res, team, man(people.size(), 0);
			for (int i = 0;i < req_skills.size();i++)
				map[req_skills[i]] = i;
			for (int i = 0;i < people.size();i++)
				for (int j = 0;j < people[i].size();j++)
					man[i] |= 1 << map[people[i][j]];
			dfs(0, req_skills.size(), man, team, res);
			return res;
		}
};
```
