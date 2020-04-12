---
title: C++ - 容器
date: 2020-03-21
tags: [C++, 编程语言]
categories: [C++]
---
在已经学习过 C 语言的前提下，学习 C++ 并用于刷算法的学习成本是非常低的，C++ 拥有丰富的 STL 标准模板库，这也是 PAT 甲级、LeeCode 等题目中经常需要用到的，用 C++ 刷算法相比于用 C 语言效率要高很多，因此学习一点 C++，对于刷算法而言大有裨益。

因此，本人总结了一些 C++ 中常用的特性，供自己平时学习时参考。
## 1. 容器
### 1.1 Vector
	特点:一个不定长的数组，它能在运行阶段设置数组的长度。
	 使用:# include <vector>
	 常用方法:
		* vector.size();
		* vector.push_back();
		* vector.resize();
		* vector.begin(); / vector.end();
	
### 1.2 set
	特点:单一的，有序的元素（从小到大）
	 使用:# include <set>
	 常用方法:
		* set.insert();
		* set.erase();
		* set.find();
		* set.begin(); / set.end();
	
#### 1.2.1 unordered_set
	特点:不排序的 set
	 使用:# include <unordered_set>

### 1.3 map
	特点:键值对，根据 key 排序（从小到大）
	使用:# include <map>
	 常用方法:
		* set.insert();
		* set.erase();
		* set.find();
		* map.begin(); / set.rbegin();
		* map.size();

#### 1.3.1 unordered_map
	特点:不排序的 map
	 使用:# include <unordered_map>
	
### 1.4 stack
	特点:堆栈，先进后出
	 使用:# include <stack>
	 常用方法:
		* stack.push();
		* stack.pop();
		* stack.top();
		* stack.size();
	
### 1.5 queue
	特点:队列，一头进，一头出，先进先出
	 使用:# include <queue>
	 常用方法:
		* queue.push();
		* queue.pop();
		* queue.front(); / queue.back();
		* queue.size();
	
## 2. 引用
```C++
void Test(int &temp){
  temp = 1;
}

void main(){
  int i = 0;
  Test(i);	// 这里修改的是 i 的值

  cout << i << endl;	// i 打印为 1
}
```
## 3. 位运算 bitset
	特点:位运算，LeetCode 常用到
	 使用:# include <bitset>
	 常用方法:
		* bitset.any();
		* bitset.none();
		* bitset.count();
		* bitset.size();
		* bitset.test();
		* bitset.set();
		* bitset.reset();
		* bitset.flip();
		* bitset.to_DataType();

## 4. String
	常用方法:
		* string.length();
		* string.substr(开始下标,几个字符（缺省默认到结尾）);

## 5. C++万能头文件
```C++
  #include <bits/stdc++.h>
```
