---
title: Algo - Recursion
date: 2022-02-26
categories: algorithms
tags:
  - algorithms
  - recursion
---

在学习`quick sort`的过程中，发现`recursion`这个概念掌握得不牢固，因此又重新学习了一遍，记录如下。<!-- more -->

## 概念

> Recursion is when a function calls itself until someone stops it. If no one stops it then it'll recurse (call itself) forever.

函数本身调用自身，直到在调用的过程中遇到一个停止调用自身的条件。一般这个停止条件会是一个 if 语句：if 条件是停止调用自身的条件，return 一个固定值。

## 解题套路

一般为 2 步：

- 第一步：写停止调用函数自身条件。

  - 思路：可以直接得到 hard code 结果的那条循环条件是一个比较好的退出条件。
    例子 1：求和。退出条件是当 argument 值为 1 时，返回值 argument 本身。
    例子 2：打印从 n 到 0 的所有整数。退出条件是 argument 值为 0 时，返回值是 argument 本身。
    例子 3: fibonacci 数列。退出条件是当 argument 小于/等于 1 时，返回值时 argument 本身。
  - 语法：一般是一条有返回值的 if 语句。
  - 技巧：
    - 如果是针对 array 进行递归的话，一般用 array 长度作为退出条件。
    - 如果是针对 number 进行递归的话，一般是最小数字。
    - 如果是针对 object 进行递归的话，一般用 `typeof obj[key] === 'object'`作为退出条件。

- 第二步：写 function 自身调用。
  - 思路：argument + function 自身。
  - 语法：简单的话，一般是一条 return 语句；复杂的话，有一些 if 条件+return 语句。

## 应用

一般可以用 for 或者 while 循环写的逻辑，基本上都可以用 recursion 改写。

- Factorial
- Fibonacci Numbers

## 练习

参考[这个 repo](https://github.com/wendyli-repos/Algorithm)里的 recursion 相关的练习。

## Refs

1. [A Quick Intro to Recursion in Javascript](https://www.freecodecamp.org/news/quick-intro-to-recursion/)
2. [Recursion and stack](https://javascript.info/recursion)
