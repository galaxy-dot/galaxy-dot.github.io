---
title: Algo - Bubble Sort
date: 2022-02-18
categories: algorithms
tags:
  - algorithms
  - sorting
  - bubbleSort
---

Learn `Bubble Sort` based on this [tutorial](https://www.youtube.com/watch?v=IAeLoGzU4RE). <!-- more -->

## Logic

Loop through an array; compare every two elements next to each other; swap position if previous one is larger than subsequent one.

## JavaScript Implement

### Basic version

```js
/**
 * Bubble Sort
 * @param {Number[]} array - The original array which is not ordered
 * @returns {Number[]}
 */
function bubbleSort(array) {
	for (let i = 0; i < array.length - 1; i++) {
		for (let j = 0; j < array.length - 1 - i; j++) {
			if (array[j] > array[j + 1]) {
				const temp = array[j];
				array[j] = array[j + 1];
				array[j + 1] = temp;
			}
		}
	}
	return array;
}
```

### Improvement version 1

To swap two elements in an array, we can also use `array destructing` in `ES6`, referring this [article](https://www.freecodecamp.org/news/array-and-object-destructuring-in-javascript/).

```js
function bubbleSort(array) {
	for (let i = 0; i < array.length - 1; i++) {
		for (let j = 0; j < array.length - 1 - i; j++) {
			if (array[j] > array[j + 1]) {
				[array[j], array[j + 1]] = [array[j + 1], array[j]];
			}
		}
	}
	return array;
}
```

There are other methods to swap two array elements in JavaScript, referring this [article](https://poopcode.com/swap-two-array-elements-in-javascript/).

### Improvement version 2

A concept to grasp before make another improvement -- `Pure Function`. Two points to note on `Pure Function`.

- Given the same input, always returns the same output
- Produces no side effects

Here we can tell the above implement satisfies first point but not second point. It mutates (which is considered as a `side effect`) the state of the parameter passed to the function. Let's check.

```js
function bubbleSort(array) {
	for (let i = 0; i < array.length - 1; i++) {
		for (let j = 0; j < array.length - 1 - i; j++) {
			if (array[j] > array[j + 1]) {
				[array[j], array[j + 1]] = [array[j + 1], array[j]];
			}
		}
	}
	return array;
}
const arr = [90, 0, -10, 88, 999, 100, 102, 2, 3, 20];
console.log(bubbleSort(arr));
console.log(arr);
console.log(bubbleSort(arr) === arr); // true
```

So, how to improve?
We can create a `shallow` copy of the parameter, mutate it and return it.

```js
function bubbleSort(array) {
	const arr = array.slice(); // create a shallow copy
	for (let i = 0; i < arr.length - 1; i++) {
		for (let j = 0; j < arr.length - 1 - i; j++) {
			if (arr[j] > arr[j + 1]) {
				[arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
			}
		}
	}
	return arr;
}
const arr = [90, 0, -10, 88, 999, 100, 102, 2, 3, 20];
console.log(bubbleSort(arr));
console.log(arr);
console.log(bubbleSort(arr) === arr); // false
```

### Improvement version 3

The above two improvements are more from cosmetic point of view. Another improvement I found online is to introduce a `flag` to check if any swap happens in the inner loop. If no swap, meaning the array is sorted, we can change `flag` value and `break` the outer loop.

```js
function bubbleSort(array) {
	let isSwapped = false;
	const arr = array.slice();
	for (let i = 0; i < arr.length - 1; i++) {
		for (let j = 0; j < arr.length - 1 - i; j++) {
			if (arr[j] > arr[j + 1]) {
				[arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
				isSwapped = true;
			}
		}
		if (isSwapped == false) break;
	}
	return arr;
}
const arr = [90, 0, -10, 100, 101, 999];
console.log(bubbleSort(arr));
```

Or using a `while` loop.

```js
function bubbleSort(arr) {
	let i = arr.length - 1; //初始时,最后位置保持不变
	while (i > 0) {
		let pos = 0; //每趟开始时,无记录交换
		for (let j = 0; j < i; j++) {
			if (arr[j] > arr[j + 1]) {
				let tmp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = tmp;
				pos = j; //记录最后交换的位置
			}
		}
		i = pos; //为下一趟排序作准备
	}
	return arr;
}
const arr = [90, 0, -10, 100, 101, 999];
console.log(bubbleSort(arr));
```

## TypeScript Rewrite

```ts
function bubbleSort(array: number[]): number[] {
	const arr = array.slice();
	for (let i = 0; i < arr.length - 1; i++) {
		for (let j = 0; j < arr.length - 1 - i; j++) {
			if (arr[j] > arr[j + 1]) {
				[arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
			}
		}
	}
	return arr;
}
const arr = [90, 0, -10, 88, 999, 100, 102, 2, 3, 20];
console.log(bubbleSort(arr));
console.log(arr);
console.log(bubbleSort(arr) === arr);
```

Note this sorting logic also works on character array, then how to annotate both types in TypeScript?
TypeScript has a concept of `generics` which quoted below:

> In TypeScript, generics are used when we want to describe a correspondence between two values. We do this by declaring a type parameter in the function signature.
> Tweaking the above a bit,

```ts
function bubbleSort<Type>(array: Type[]): Type[] {
	const arr = array.slice();
	let isSwapped = false;
	for (let i = 0; i < arr.length - 1; i++) {
		for (let j = 0; j < arr.length - 1 - i; j++) {
			if (arr[j] > arr[j + 1]) {
				[arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
				isSwapped = true;
			}
		}
		if (isSwapped == false) break;
	}
	return arr;
}
const charArr = ['a', 'c', 'b'];
const numArr = [90, 0, -10, 88, 999, 100, 102, 2, 3, 20];
console.log(bubbleSort(charArr));
console.log(bubbleSort(numArr));
```

## Miscellaneous

### Algo merits - copied from [here](https://www.geeksforgeeks.org/bubble-sort/)

Worst and Average Case Time Complexity: O(n\*n). Worst case occurs when array is reverse sorted.
Best Case Time Complexity: O(n). Best case occurs when array is already sorted.
Auxiliary Space: O(1)
Boundary Cases: Bubble sort takes minimum time (Order of n) when elements are already sorted.
Sorting In Place: Yes
Stable: Yes

The meaning of `stable` is if there are element with same value, their sequence in the result and in the parameter is unchanged thus `stable`.

## Use case

Suitable for sort an array which is almost sorted. Not very practical in real-life business transactions.

## Refs

1. [面试官：说说你对冒泡排序的理解？如何实现？应用场景？](https://developer.51cto.com/article/684737.html)
2. [Bubble Sort](https://www.geeksforgeeks.org/bubble-sort/)
3. [面试官：手写一个冒泡排序，并对其改进](https://zhuanlan.zhihu.com/p/81395118)
