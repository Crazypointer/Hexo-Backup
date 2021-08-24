---
title: Safe Or Unsafe - HDU - 2527 (哈夫曼树 + 优先队列)
tags:
	算法
	ACM
excerpt: Javac++ 一天在看计算机的书籍的时候，看到了一个有趣的东西！每一串字符都可以被编码成一些数字来储存信息，但是不同的编码方式得到的储存空间是不一样的！并且当储存空间大于一定的值的时候是不安全的！所以Javac++ 就想是否有一种方式是可以得到字符编码最小的空间值！显然这是可以的，因为书上有这一块内容--哈夫曼编码(Huffman Coding)；一个字母的权值等于该字母在字符串中出现的频率。所以Javac++ 想让你帮忙，给你安全数值和一串字符串，并让你判断这个字符串是否是安全的？
---




原题链接：[点击进入](http://acm.hdu.edu.cn/showproblem.php?pid=2527)

## 问题描述：

Javac++ 一天在看计算机的书籍的时候，看到了一个有趣的东西！每一串字符都可以被编码成一些数字来储存信息，但是不同的编码方式得到的储存空间是不一样的！并且当储存空间大于一定的值的时候是不安全的！所以Javac++ 就想是否有一种方式是可以得到字符编码最小的空间值！显然这是可以的，因为书上有这一块内容--哈夫曼编码(Huffman Coding)；一个字母的权值等于该字母在字符串中出现的频率。所以Javac++ 想让你帮忙，给你安全数值和一串字符串，并让你判断这个字符串是否是安全的？

## Input：

输入有多组case，首先是一个数字n表示有n组数据，然后每一组数据是有一个数值m(integer)，和一串字符串没有空格只有包含小写字母组成！

## Output：

如果字符串的编码值小于等于给定的值则输出yes，否则输出no。

## Sample Input：

```c
2
12
helloworld
66
ithinkyoucandoit
```

## Sample Output：

```c
no
yes
```

哈夫曼树模板题，直接判断字符串的WPL值是否超过给定安全值即可。由于优先队列是默认以升序规则(less)排列，每次取数是取出的最大的那个数，所以我们在定义有限队列的时候，将优先队列中的排序规则换成greater.

```c
#include <stdio.h>
#include<string.h> 
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
const int MAX = 1e3+5;
char ch[MAX];
int cnt[50];

int main()
{
	int n;
	scanf("%d", &n);
	priority_queue< int,vector<int>,greater<int> > q;//创建优先队列   //greater对队列进行降序排序 实现小根堆 
	while(n--)//测试样例的组数
	{
		int m, len,mm;
		scanf("%d%s",&m,&ch);
		len = strlen(ch);
		for(int i = 0;i < len; ++i) //统计字符串中字母出现的频率 
		{
			cnt[ch[i] - 'a']++;
		}

		for(int i = 0; i < 26; ++i)//将统计的字母出现的次数放进队列中 
		{
			if(cnt[i]){
			q.push(cnt[i]);
			}
		}
	
		int wpl=0;
		while(q.size()>1){
			int min1=q.top();
			q.pop();
			int min2=q.top();
			q.pop();
			wpl+=min1+min2;
			q.push(min1+min2);
		}
		if(wpl==0){
			wpl=q.top();
		}
		
		if(wpl<=m){
			printf("yes\n");
		}
		else{
			printf("no\n"); 
		}
		while(!q.empty())//清空队列 
		{
			q.pop();
		}
		memset(cnt,0,sizeof(cnt));
	}
	return 0;
} 
```

