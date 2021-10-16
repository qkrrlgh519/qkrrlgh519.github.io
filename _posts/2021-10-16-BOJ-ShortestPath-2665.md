---
layout: post
title: "BOJ[2665] - 미로만들기 by JavaScript"
date: 2021-10-16 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 미로만들기

## 문제

- [백준 2665번 - 미로만들기](https://www.acmicpc.net/problem/2665)

## 언어

- JavaScript

## 순서도

1. 최소 힙 구현
2. 다익스트라 알고리즘 함수 구현
3. 주어진 그래프에서 흰 방은 비용을 0 으로, 검은 방은 비용을 1 로 간주해서 다익스트라 알고리즘 적용
4. 시작방에서 끝방에 도달하는데 걸리는 최소 비용 출력

## 문제 풀이 step 1

- n x n 바둑판 모양으로 총 `n ^ 2` 개의 방이 있습니다.
- 일부분은 검은 방이고 나머지는 모두 흰 방입니다.
- 검은 방은 사면이 벽으로 싸여 있어 들어갈 수 없습니다.
- 서로 붙어 있는 두 개의 흰 방 사이에는 문이 있어서 지나다닐 수 있습니다.
- 윗줄 맨 왼쪽 방은 시작방으로서 항상 흰 방이고, 아랫줄 맨 오른쪽 방은 끝방으로서 역시 흰 방입니다.
- 시작방에서 출발하여 길을 찾아서 끝방으로 가는 것이 목적인데, 아래 그림의 경우에는 시작방에서 끝 방으로 갈 수가 없습니다.
- 부득이 검은 방 몇 개를 흰 방으로 바꾸어야 하는데 되도록 적은 수의 방의 색을 바꾸고 싶습니다.
- 이 때, 시작방에서 끝방으로 가기 위해서 색을 바꾸어야 하는 방의 최소 개수를 구하는 문제입니다.

## 문제 풀이 step 2

- 본 문제는 다익스트라 알고리즘을 이용하면 풀 수 있습니다.
- `n ^ 2` 개의 방에서 흰방은 비용을 0 으로, 검은방은 비용을 1 로 설정합니다.
- 우선, 최소 힙을 구현하고, `n ^ 2` 개의 방을 그래프로 간주합니다.
- 시작방은 [0, 0] 에서 상, 하, 좌, 우 이렇게 4 개의 방향 중에 갈 수 있는 방으로 이동합니다.
- 이와 동시에 distance 배열을 갱신하고, 최소 힙에 다음 방을 넣는 방식으로 다익스트라 알고리즘을 적용합니다.
- 마지막으로 [n - 1][n - 1] 까지 최소 비용(최소 개의 방의 색 변환)이 드는 경로를 구하고, 최소 비용(최소 개의 방의 색 변환)을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 문제를 처음 접했을 때, "이 문제가 다익스트라 알고리즘이랑 무슨 상관이 있지?" 라는 생각을 했었습니다.
- 검은방을 비용이 있는 정점으로 생각해보니, 다익스트라 알고리즘이 상당히 적합하다는 것을 깨달을 수 있었습니다.
- 또한 다익스트라 알고리즘이 활용성이 높은 알고리즘이라는 것도 깨달을 수 있었습니다.
- 물론, 본 문제는 BFS 알고리즘으로도 풀 수 있습니다. 아마 다익스트라 알고리즘을 몰랐다면, BFS 알고리즘으로 풀었을 것 같습니다.
- 아래에 BFS 알고리즘으로 푼 소스코드도 남깁니다.

## 소스 코드

- 다익스트라 알고리즘 적용

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const INF = Number.MAX_SAFE_INTEGER;

// 4 개의 방향
const dir = [
	[-1, 0],
	[1, 0],
	[0, -1],
	[0, 1],
];

const isPossibleRoute = (n, x, y) => 0 <= x && x < n && 0 <= y && y < n;

// 최소 힙 구현
class MinHeap {
	constructor() {
		this.bucket = [null];
	}

	insert(item) {
		let index = this.bucket.length;

		while (index !== 1 && item[0] < this.bucket[parseInt(index / 2)][0]) {
			this.bucket[index] = this.bucket[parseInt(index / 2)];
			index = parseInt(index / 2);
		}

		this.bucket[index] = item;
	}

	delete() {
		if (this.isEmpty()) return;

		let parentIdx = 1;
		let childIdx = 2;
		let item = this.bucket[1];
		let lastItem = this.bucket.pop();

		while (childIdx < this.bucket.length) {
			if (
				childIdx + 1 < this.bucket.length &&
				this.bucket[childIdx][0] > this.bucket[childIdx + 1][0]
			) {
				childIdx += 1;
			}

			if (lastItem[0] <= this.bucket[childIdx][0]) break;

			this.bucket[parentIdx] = this.bucket[childIdx];

			parentIdx = childIdx;
			childIdx *= 2;
		}

		if (!this.isEmpty()) this.bucket[parentIdx] = lastItem;

		return item;
	}

	isEmpty() {
		return this.bucket.length === 1;
	}
}

// 다익스트라 알고리즘 함수
const dijkstra = (n, graph, start) => {
	const minHeap = new MinHeap();
	const distance = Array.from({length: n}, () => Array(n).fill(INF));

	// 시작방 갱신
	minHeap.insert([0, start]);
	distance[start[0]][start[1]] = 0;

	while (!minHeap.isEmpty()) {
		const [fromCost, [fx, fy]] = minHeap.delete();

		// distance 배열에 기록된 비용보다 큰 경로는 무시
		if (distance[fx][fy] < fromCost) continue;

		// 연결된 정점들(상, 하, 좌, 우)을 모두 확인해보면서
		for (let i = 0; i < 4; i++) {
			const [tx, ty] = [fx + dir[i][0], fy + dir[i][1]];
			if (!isPossibleRoute(n, tx, ty)) continue;

			const toCost = fromCost + graph[tx][ty];

			// 비용이 더 작은 경로가 있다면 distance 배열 갱신 하고, 최소 힙에 넣기
			if (toCost < distance[tx][ty]) {
				distance[tx][ty] = toCost;
				minHeap.insert([toCost, [tx, ty]]);
			}
		}
	}

	return distance;
};

const solution = (input) => {
	const n = Number(input[0]);
	const graph = input
		.slice(1)
		.map((v) => v.split("").map((v) => (v === "1" ? 0 : 1)));

	const distance = dijkstra(n, graph, [0, 0]);
	return distance[n - 1][n - 1];
};

console.log(solution(input));
```

## 소스 코드 2

- BFS 알고리즘 적용

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const INF = Number.MAX_SAFE_INTEGER;

const dir = [
	[-1, 0],
	[1, 0],
	[0, -1],
	[0, 1],
];

const isPossibleRoute = (n, x, y) => 0 <= x && x < n && 0 <= y && y < n;

// 큐 생성
class Queue {
	constructor() {
		this.bucket = [];
		this.front = -1;
		this.rear = -1;
	}

	enqueue(item) {
		this.bucket[++this.rear] = item;
	}

	dequeue() {
		return this.bucket[++this.front];
	}

	isEmpty() {
		return this.front === this.rear;
	}
}

// bfs 알고리즘 구현
const bfs = (n, graph, start) => {
	// 방문처리를 대신할 distance 배열 생성
	const distance = Array.from({length: n}, () => Array(n).fill(-1));

	let queue = new Queue();
	let nextQueue = new Queue();

	distance[start[0]][start[1]] = 0;
	queue.enqueue([start[0], start[1]]);

	let depth = 0;
	while (!queue.isEmpty()) {
		const [fx, fy] = queue.dequeue();

		for (let i = 0; i < 4; i++) {
			const [tx, ty] = [fx + dir[i][0], fy + dir[i][1]];

			if (!isPossibleRoute(n, tx, ty)) continue;
			if (distance[tx][ty] !== -1) continue;

			if (graph[tx][ty] === 1) {
				// 검은 방을 접하면 다음 큐에 넣기
				distance[tx][ty] = depth + 1;
				nextQueue.enqueue([tx, ty]);
			} else {
				// 흰 방을 접하면 현재 큐에 넣기
				distance[tx][ty] = depth;
				queue.enqueue([tx, ty]);
			}
		}

		if (queue.isEmpty()) {
			// 현재 큐가 다 비었다면, 다음 큐로 갱신
			queue = nextQueue;
			nextQueue = new Queue();
			depth += 1;
		}
	}

	return distance;
};

const solution = (input) => {
	const n = Number(input[0]);
	const graph = input
		.slice(1)
		.map((v) => v.split("").map((v) => (v === "1" ? 0 : 1)));

	const distance = bfs(n, graph, [0, 0]);

	return distance[n - 1][n - 1];
};

console.log(solution(input));
```
