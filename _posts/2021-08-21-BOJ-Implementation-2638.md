---
layout: post
title: "BOJ[2638] - 치즈 by JavaScript"
date: 2021-08-21 12:00:00 +0900
categories: BOJ(Implementation)
---

# 치즈

## 문제

- [백준 2638번 - 치즈](https://www.acmicpc.net/problem/2638)

## 언어

- JavaScript

## 순서도

1. 치즈의 위치 모두 찾기
2. 모눈 종이에 공기 표시하기
3. 지울 치즈의 위치 모두 찾기
4. 치즈 지우기
5. 모든 치즈가 지워질 때까지 2, 3, 4 번을 반복하며 시간 계산하기

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- N x M 크기의 모눈종이 위에 치즈가 있습니다.
  - N 은 세로 격자의 수, M 은 가로 격자의 수를 의미합니다.
  - 모눈종이 모양의 치즈에서 각 치즈 격자(작은 정사각형 모양)의 4변 중에서 적어도 2변 이상이 공기와 접촉하면 한 시간만에 녹아 없어집니다.
  - 치즈 내부에 있는 공간은 치즈 외부 공기와 접촉하지 않은 것으로 가정합니다.
  - 모눈종이의 맨 가장자리에는 치즈가 놓이지 않는 것으로 가정합니다.
- 위와 같은 모눈종이와 치즈에 대한 정보가 주어질 때 입력으로 주어진 치즈가 모두 녹아 없어지는데 걸리는 정확한 시간을 구하는 문제입니다.

## 문제 풀이 step 2

- 우선, 모눈종이에서 치즈의 위치를 모두 찾습니다.
- 모눈종이에서 공기의 위치를 표시하고, 치즈를 지우고, 시간을 셉니다.
  - 처음에는 가장 위쪽, 가장 왼쪽인 [0, 0] 좌표에서 BFS 탐색을 통해서 모눈종이에 공기를 표시합니다.
    - 모눈종이의 맨 가장자리에는 치즈가 놓이지 않으니 [0, 0] 좌표에서 BFS 탐색을 시작합니다.
  - 아까 구한 치즈의 위치를 이용해서 각 치즈가 녹아 없어질 치즈인지 검사합니다.
    - 각 치즈마다 상, 하, 좌, 우로 4방향을 검사해서 공기에 접촉한 면이 2면 이상이면 녹아 없어질 치즈입니다.
  - 녹아 없어질 치즈를 모두 지우고, 시간을 1 시간 증가시킵니다.
- 다시, 모눈종이에서 공기의 위치를 표시하고, 치즈를 지우고, 시간을 셉니다.
  - 이번에는 전 단계에서 녹아 없어진 모든 치즈의 위치에서 BFS 탐색을 통해서 모눈종이에 공기를 표시합니다.
  - 치즈의 위치를 이용해서 각 치즈가 녹아 없어질 치즈인지 검사합니다.
  - 녹아 없어질 치즈를 모두 지우고, 시간을 1 시간 증가시킵니다.
- 모눈종이에서 모든 치즈가 녹아 없어질 때까지 위 과정을 반복합니다.
- 반복이 종료되면, 소요된 시간을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 상, 하, 좌, 우
const cx = [0, 0, 1, -1];
const cy = [1, -1, 0, 0];

class Dot {
	constructor(x, y) {
		this.x = x;
		this.y = y;
	}
}

// 간단한 Queue 구현
class Queue {
	constructor() {
		this.bucket = [];
		this.front = -1;
		this.rear = -1;
	}

	enqueue(data) {
		this.bucket[++this.rear] = data;
	}

	dequeue() {
		return this.bucket[++this.front];
	}

	isEmpty() {
		return this.front === this.rear;
	}
}

// 탐색할 좌표가 모눈종이를 벗어나는지 검사하는 함수
isPossibleRoute = (n, m, dot) =>
	0 <= dot.x && dot.x < n && 0 <= dot.y && dot.y < m;

// BFS 탐색을 통해서 모눈종이에서 공기인 부분을 표시
const BFS = (n, m, grid, air, x, y) => {
	const queue = new Queue();

	queue.enqueue(new Dot(x, y));
	air[x][y] = true;
	while (!queue.isEmpty()) {
		const from = queue.dequeue();

		for (let i = 0; i < 4; i++) {
			const to = new Dot(from.x + cx[i], from.y + cy[i]);

			if (!isPossibleRoute(n, m, to)) continue;
			if (grid[to.x][to.y] === 1) continue;
			if (air[to.x][to.y]) continue;

			air[to.x][to.y] = true;
			queue.enqueue(to);
		}
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	const grid = input.slice(1).map((v) => v.split(" ").map(Number));

	// 모든 치즈의 위치 찾기
	let cheeses = [];
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < m; j++) {
			if (grid[i][j] === 0) continue;
			cheeses.push(new Dot(i, j));
		}
	}

	let hour = 0;
	let erase = [new Dot(0, 0)];
	const air = Array.from({length: n}, () => Array(m).fill(false));
	while (cheeses.length > 0) {
		// 모눈 종이에 공기 표시
		for (let dot of erase) BFS(n, m, grid, air, dot.x, dot.y);

		// 녹아 없어질 치즈의 위치 찾기
		const nextErase = [];
		const nextCheeses = [];
		for (let dot of cheeses) {
			let cnt = 0;
			for (let i = 0; i < 4; i++) {
				const nextDot = new Dot(dot.x + cx[i], dot.y + cy[i]);
				if (!isPossibleRoute(n, m, nextDot)) continue;

				if (air[nextDot.x][nextDot.y]) cnt += 1;
			}

			// 공기와 접촉한 면이 2 이상이면 녹아 없어질 치즈
			if (cnt >= 2) {
				nextErase.push(dot);
			} else {
				nextCheeses.push(dot);
			}
		}

		// 녹아 없어질 치즈 지우기
		for (let dot of nextErase) {
			grid[dot.x][dot.y] = 0;
		}

		erase = nextErase;
		cheeses = nextCheeses;

		// 1 시간 증가
		hour += 1;
	}

	return hour;
};

console.log(solution(input));
```
