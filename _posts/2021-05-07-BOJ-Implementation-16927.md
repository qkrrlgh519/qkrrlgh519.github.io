---
layout: post
title: "BOJ[16927] - 배열 돌리기 2 by JavaScript"
date: 2021-05-07 02:00:00 +0900
categories: BOJ(Implementation)
---

# 배열 돌리기 2

## 문제

- [백준 16927번 - 배열 돌리기 2](https://www.acmicpc.net/problem/16927)

## 언어

- JavaScript

## 문제 풀이 step 1

- 구현 문제로서, 문제에서 주어진 설명대로 구현하는 문제입니다.
- [백준 16926번 - 배열 돌리기 1 풀이](<https://qkrrlgh519.github.io/boj(implementation)/2021/05/07/BOJ-Implementation-16926.html>) 와 완전 똑같은 방식으로 풀 수 있습니다.
- 따라서 본 포스트에는 풀이를 적지 않고, 소스 코드만 작성해놓았습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

class Queue {
	constructor() {
		this.bucket = [];
		this.front = -1;
		this.rear = -1;
	}

	isEmpty() {
		return this.front === this.rear;
	}

	enqueue(data) {
		this.bucket[++this.rear] = data;
	}

	dequeue() {
		return this.bucket[++this.front];
	}

	size() {
		return this.rear - this.front;
	}
}

const rotate = (k, l, n, m, r, arr, resArr) => {
	const queue = new Queue();

	// 좌상단, 좌하단, 우상단, 우하단
	let [ltx, lty] = [k, l];
	let [lbx, lby] = [n - 1, l];
	let [rtx, rty] = [k, m - 1];
	let [rbx, rby] = [n - 1, m - 1];

	// 그룹화
	for (let i = lty; i < rty; i++) {
		queue.enqueue(arr[ltx][i]);
	}
	for (let i = rtx; i < rbx; i++) {
		queue.enqueue(arr[i][rty]);
	}
	for (let i = rby; i > lby; i--) {
		queue.enqueue(arr[rbx][i]);
	}
	for (let i = lbx; i > ltx; i--) {
		queue.enqueue(arr[i][lby]);
	}

	// 불필요한 회전수 제거
	let count = r % queue.size();

	// 회전
	while (count-- > 0) {
		queue.enqueue(queue.dequeue());
	}

	// 그룹을 다시 배열에 복사
	for (let i = lty; i < rty; i++) {
		arr[ltx][i] = queue.dequeue();
	}
	for (let i = rtx; i < rbx; i++) {
		arr[i][rty] = queue.dequeue();
	}
	for (let i = rby; i > lby; i--) {
		arr[rbx][i] = queue.dequeue();
	}
	for (let i = lbx; i > ltx; i--) {
		arr[i][lby] = queue.dequeue();
	}
};

const solution = (input) => {
	let [n, m, r] = input[0].split(" ").map(Number);
	let arr = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	let [k, l] = [0, 0];
	const resArr = Array.from({length: n}, () => []);
	while (n - k !== 0 && m - l !== 0) {
		rotate(k, l, n, m, r, arr, resArr);
		[k, l, n, m] = [k + 1, l + 1, n - 1, m - 1];
	}

	return arr.map((v) => v.join(" ")).join("\n");
};

console.log(solution(input));
```
