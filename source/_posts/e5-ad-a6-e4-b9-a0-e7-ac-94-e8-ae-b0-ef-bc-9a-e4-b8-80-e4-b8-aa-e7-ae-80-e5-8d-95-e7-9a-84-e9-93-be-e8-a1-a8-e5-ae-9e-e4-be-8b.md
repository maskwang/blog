---
title: 学习笔记：一个简单的链表实例
id: 577
categories:
  - C语言
  - 技术专栏
  - 编程语言
date: 2012-07-11 13:54:15
tags:
---

<pre lang="c">
/* 使用结构链表 */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define TSIZE 45 /* 存放片名的数组大小 */

struct film
{
	char title[TSIZE];
	int rating;
	struct film * next; /* 指向链表的下一个结构 */
};

int main(void)
{
	struct film * head = NULL;
	struct film * prev, * current;
	char input[TSIZE];

	/* 收集并存储信息 */
	puts("Enter first movie title: ");
	while(gets(input) != NULL && input[0] != '\0')
	{
		current = (struct film *)malloc(sizeof(struct film));
		if (head == NULL)
			head = current;
		else
			prev->next = current;
		current->next = NULL;
		strcpy(current->title, input);
		puts("Enter your rating <0-10>: ");
		scanf("%d", &current->rating);
		while(getchar() != '\n')
			continue;
		puts("Enter next movie title (empty line to stop): ");
		prev = current;
	}

	/* 给出电影列表 */
	if (head == NULL)
		printf("No data entered. ");
	else
		printf("Here is the movie list: \n ");
	current = head;
	while(current != NULL)
	{
		printf("Movie: %s Rating: %d \n", current->title, current->rating);
		current = current->next;
	}

	/* 任务已完成，因此释放所有分配的内存 */
	current = head;
	while(current != NULL)
	{
		free(current);
		current = current->next;
	}
	printf("Bye! \n ");
	getchar();
	return 0;
}
</pre>