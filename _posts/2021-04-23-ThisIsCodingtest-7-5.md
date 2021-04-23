---
layout: post
title: "이것이 코딩테스트다 - [BinarySearch] 부품 찾기 by JavaScript"
date: 2021-04-23 16:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 부품 찾기

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 주어진 N 개의 부품을 오름 차순으로 정렬하고, 손님이 문의한 M 개의 부품을 target 으로 해서 이진 탐색을 수행하면 간단하게 해결할 수 있습니다.
- 이 문제에 한해서는 이진 탐색 보다 계수 정렬을 응용해서 더 빠르게 풀 수 있습니다.
  - 각 부품이 존재하는지 여부를 판단하는 부품 배열을 생성 후, 그 부품 배열에 접근해서 손님이 문의한 부품이 있는지 판단하는 방식으로 해결합니다.

## 소스 코드 (이진 탐색)

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `5
// 8 3 7 9 2
// 3
// 5 7 9`.split("\n");

const binarySearch = (arr, target, start, end) => {
	while (start <= end) {
		const mid = parseInt((start + end) / 2);
		if (arr[mid] === target) return mid;

		if (arr[mid] > target) end = mid - 1;
		else start = mid + 1;
	}

	return null;
};

const solution = (input) => {
	const n = Number(input[0]);
	const arr = input[1]
		.split(" ")
		.map(Number)
		.sort((a, b) => a - b);
	const k = Number(input[2]);
	const want = input[3].split(" ").map(Number);

	let res = "";
	for (let i = 0; i < k; i++) {
		if (binarySearch(arr, want[i], 0, n - 1) !== null) res += "yes ";
		else res += "no ";
	}

	return res;
};

console.log(solution(input));
```

## 소스 코드 (계수 정렬 응용)

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `5
// 8 3 7 9 2
// 3
// 5 7 9`.split("\n");

const countSort = (arr, n) => {
	const countArr = [];

	for (let i = 0; i < n; i++) {
		const key = arr[i];
		if (countArr[key] === undefined) countArr[key] = 0;
		countArr[key] += 1;
	}

	return countArr;
};

const solution = (input) => {
	const n = Number(input[0]);
	const arr = input[1]
		.split(" ")
		.map(Number)
		.sort((a, b) => a - b);
	const k = Number(input[2]);
	const want = input[3].split(" ").map(Number);

	const countArr = countSort(arr, n);
	let res = "";
	for (let i = 0; i < k; i++) {
		if (countArr[want[i]] !== undefined) res += "yes ";
		else res += "no ";
	}

	return res;
};

console.log(solution(input));
```
