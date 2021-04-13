---
layout: post
title: "이것이 코딩테스트다 - [DFS/BFS] 미로 탈출 by JavaScript"
date: 2021-04-13 12:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 미로 탈출

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- BFS 탐색을 통해서 풀 수 있는 문제입니다.
- BFS 탐색은 현재 위치에서 갈 수 있는 모든 노드를 한 단계만큼 탐색, 그리고 다음 위치에서 갈 수 있는 모든 노드를 한 단계만큼 탐색하는 방식으로 동작하기 때문에 최단 거리를 구하는 알고리즘에 사용됩니다.
- 이 문제에서도 동빈이가 현재 위치에서 미로의 출구까지의 최단 거리를 구하라고 했으므로, BFS 탐색을 이용할 수 있는 것입니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `5 6
// 101010
// 111111
// 000001
// 111111
// 111111`.split("\n");

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
				arr[to.x][to.y] === 0 ||
				visited[to.x][to.y]
			)
				continue;

			visited[to.x][to.y] = true;
			arr[to.x][to.y] = arr[from.x][from.y] + 1;
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

	const visited = Array.from({length: n}, () => Array(m).fill(false));
	BFS(n, m, arr, visited, new Dot(0, 0));

	return arr[n - 1][m - 1];
};

console.log(solution(input));
```
