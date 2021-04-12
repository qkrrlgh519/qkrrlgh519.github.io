---
layout: post
title: "이것이 코딩테스트다 - [DFS/BFS] 음료수 얼려 먹기 by JavaScript"
date: 2021-04-12 17:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 음료수 얼려 먹기

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- DFS 나 BFS 탐색을 통해서 풀 수 있는 문제입니다.
- 저자님께서는 DFS 탐색으로 푸는 방법에 대해서 작성해주셨고, 저는 BFS 탐색으로 풀어봤습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `4 5
// 00110
// 00011
// 11111
// 00000`.split("\n");

// const input = `15 14
// 00000111100000
// 11111101111110
// 11011101101110
// 11011101100000
// 11011111111111
// 11011111111100
// 11000000011111
// 01111111111111
// 00000000011111
// 01111111111000
// 00011111111000
// 00000001111000
// 11111111110011
// 11100011111111
// 11000111111111`.split("\n");

class Dot {
	constructor(x, y) {
		this.x = x;
		this.y = y;
	}
}

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
		this.bucket[++this.front] = data;
	}

	dequeue() {
		if (!this.isEmpty()) return this.bucket[++this.rear];
	}
}

const isPossibleRoute = (n, m, to) =>
	0 <= to.x && to.x < n && 0 <= to.y && to.y < m;

const BFS = (n, m, arr, visited, start) => {
	const cx = [0, 0, 1, -1];
	const cy = [1, -1, 0, 0];

	const queue = new Queue();
	visited[start.x][start.y] = true;
	queue.enqueue(start);

	while (!queue.isEmpty()) {
		const from = queue.dequeue();

		for (let i = 0; i < 4; i++) {
			const to = new Dot(from.x + cx[i], from.y + cy[i]);
			if (
				!isPossibleRoute(n, m, to) ||
				arr[to.x][to.y] === 1 ||
				visited[to.x][to.y]
			)
				continue;

			visited[to.x][to.y] = true;
			queue.enqueue(to);
		}
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	const arr = [];
	for (let i = 1; i < n + 1; i++) {
		arr[i - 1] = input[i].split("").map(Number);
	}

	let cnt = 0;
	const visited = Array.from({length: n}, () => Array(m).fill(false));
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < m; j++) {
			if (arr[i][j] === 1 || visited[i][j]) continue;

			BFS(n, m, arr, visited, new Dot(i, j));
			cnt++;
		}
	}

	return cnt;
};

console.log(solution(input));
```
