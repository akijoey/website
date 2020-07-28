---
title: LeetCode - 430
date: 2019-11-06
tags: LeetCode
categories: Algorithm
keywords: leetcode430
description: leetcode430
cover: /img/cover_posts/acm.jpg
top_img: /img/cover_posts/acm.jpg
---
## Problem

题目链接: [https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

![](/img/img_posts/leetcode430.png)

## Write Up

简单的双链表插入操作.
将子链表插入父链表中.
next 指针用来暂存父表的下一项.
注意 next 有可能为 nullptr.

## Source Code

``` c++
/*Flatten a Multilevel Doubly Linked List*/
class Solution
{
	public:
		Node* flatten(Node* head)
		{
			if (head == nullptr)
				return nullptr;
			for (Node* p = head;p != nullptr;p = p->next)
			{
				Node* next = p->next;
				if (p->child != nullptr)
				{
					p->next = p->child;
					p->child->prev = p;
					p->child = nullptr;
					Node* child = p->next;
					while (child->next != nullptr)
						child = child->next;
					child->next = next;
					if (next != nullptr)
						next->prev = child;
				}
			}
			return head;
		}
};
```
