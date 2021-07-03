---
layout: post
title: "BOJ[2636] - 치즈 by JavaScript"
date: 2021-07-03 14:00:00 +0900
categories: BOJ(Implementation)
---

# 치즈

## 문제

- [백준 2636번 - 치즈](https://www.acmicpc.net/problem/2636)

## 언어

- JavaScript

## 순서도

1. 가장 왼쪽이며, 가장 위쪽에 있는 좌표를 시작점 설정
2. 설정한 시작점을 기준으로 치즈가 없는 칸을 모두 queue 에 넣고, 치즈가 있는 칸은 nextQueue 에 넣으며 BFS 탐색 진행
3. nextQueue 를 queue 로 옮기고 BFS 탐색을 진행, 이번에는 queue 에는 어떤 칸도 넣지 않고, nextQueue 에만 치즈가 있는 칸 넣기
4. nextQueue 에 넣을 칸이 없을 때까지 3 번 과정을 반복을 반복하며 단계적으로 BFS 탐색 진행
5. 탐색을 진행하는 과정 속에서, 소요 시간을 기록하고, 각 단계별 녹는 치즈조각을 구별
6. 소요 시간과 모두 녹기 한 시간 전에 남아있는 치즈조각의 개수 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 사각형 모양의 판에 치즈가 있고, 시간이 지남에 따라 치즈는 녹습니다.
- 사각형 모양의 판에 관한 정보는 다음과 같습니다.
  - 판의 가장자리에는 치즈가 놓여있지 않습니다.
  - 판에는 한 조각의 치즈가 놓여있습니다.
- 치즈에 관한 정보는 다음과 같습니다.
  - 치즈에는 하나 이상의 구멍이 있을 수 있습니다.
  - 공기와 접촉된 칸에 놓인 치즈는 한 시간이 지나면 녹아 없어집니다.
  - 치즈의 구멍 속에는 공기가 없지만 구멍을 둘러싼 치즈가 녹아서 구멍이 열리면 구멍 속으로 공기가 들어옵니다.
- 위의 정보들을 이용해서 시간에 따라 치즈의 변화를 판에 적용시키고, 최종적으로 **공기 중에서 치즈가 모두 녹아 없어지는데 걸리는 시간**과 **모두 녹기 한 시간 전에 남아있는 치즈 조각이 놓여있는 칸의 개수**를 출력하는 문제입니다.

## 문제 풀이 step 2

- 본 문제는 Queue 두 개(`queue`, `nextQueue`)를 이용해서 단계별로 BFS 탐색을 진행해야 하는 문제입니다.
  - 왜냐하면, 치즈는 겉에서 부터 한 시간씩 한 꺼풀씩 녹기 때문입니다.
- 최종적으로 구해야 하는 것은 두 가지입니다.
  - **공기 중에서 치즈가 모두 녹아 없어지는 데 걸리는 시간** : 한 꺼풀이 녹고 다음 BFS 탐색을 진행할 때마다 소요 시간을 증가시킵니다.
  - **모두 녹기 한 시간 전에 남아있는 치즈 조각이 놓여있는 칸의 개수** : 별도의 배열(`distance[]`)을 생성해서, 각 단계별로 소요 시간을 기록합니다.
- 우선, 판에서 치즈가 놓여있지 않은 칸들에만 BFS 탐색을 진행합니다.
  - 동시에 치즈가 놓여있는 칸들은 다음 BFS 탐색을 위해서 `nextQueue` 에 넣어줍니다.
- `nextQueue` 에 저장한 치즈가 놓여있는 칸들을 `queue` 에 옮겨줍니다.
- 그리고 `queue` 에서 BFS 탐색을 진행합니다. 이때, 소요 시간을 1 증가시키고, 별도의 배열(`distance[]`)에서 탐색한 칸들에 대해서 소요 시간을 기록해줍니다.
  - 동시에 치즈가 놓여있는 칸들은 다음 BFS 탐색을 위해서 `nextQueue` 에 넣어줍니다.
- 치즈가 다 녹아서 더 이상 `nextQueue` 에 넣을 치즈가 없을 때까지 BFS 탐색을 진행합니다.
- BFS 탐색을 진행하며 기록했던 소요 시간을 출력합니다.
- 그리고 별도의 배열(`distance[]`)을 이용해서 모두 녹기 한 시간 전에 남아있는 치즈 조각이 놓여있는 칸의 개수를 세서 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

class Dot {
	constructor(x, y) {
		this.x = x;
		this.y = y;
	}
}

// BFS 탐색을 위해서 별도의 Queue 생성, 배열을 사용해도 무방
class Queue {
	constructor() {
		this.bucket = [];
		this.rear = -1;
		this.front = -1;
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

// 다음으로 탐색할 칸이 판을 벗어나지 않는지 판단하는 함수
const isPossibleRoute = (n, m, to) =>
	0 <= to.x && to.x < n && 0 <= to.y && to.y < m;

const BFS = (n, m, arr, queue, distance, nextQueue, time) => {
	// 네 방향 (상, 하, 좌, 우)
	const cx = [0, 0, 1, -1];
	const cy = [1, -1, 0, 0];

	while (!queue.isEmpty()) {
		const from = queue.dequeue();

		for (let i = 0; i < 4; i++) {
			const to = new Dot(from.x + cx[i], from.y + cy[i]);

			if (!isPossibleRoute(n, m, to)) continue;
			if (distance[to.x][to.y] !== -1) continue;

			// 치즈가 놓여있는 칸이라면 nextQueue 에 넣어주고, 소요시간 기록해주기
			if (arr[to.x][to.y] === 1) {
				distance[to.x][to.y] = time;
				nextQueue.enqueue(to);
			}

			// 치즈가 놓여있지 않은 칸이라면 queue 에 넣어주고, 소요시간 기록해주기
			if (arr[to.x][to.y] === 0) {
				distance[to.x][to.y] = 0;
				queue.enqueue(to);
			}
		}
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	const arr = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	let queue = new Queue();
	queue.enqueue(new Dot(0, 0));
	let nextQueue = new Queue();
	const distance = Array.from({length: n}, () => Array(m).fill(-1));
	distance[0][0] = 0;
	let time = 1;
	while (true) {
		// BFS 탐색 진행
		BFS(n, m, arr, queue, distance, nextQueue, time);

		// 다 녹아서 더 이상 탐색할 치즈가 없다면 종료
		if (nextQueue.isEmpty()) break;

		// nextQueue 의 내용을 queue 로 옮겨주기
		queue = nextQueue;
		nextQueue = new Queue();

		// 소요시간 1 증가
		time += 1;
	}

	time -= 1;
	let lastPieces = 0;
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < m; j++) {
			// 모두 녹기 한 시간 전에 남아있는 치즈조각이 놓여 있는 칸의 개수를 세서 출력
			if (distance[i][j] === time) lastPieces += 1;
		}
	}

	return `${time}\n${lastPieces}`;
};

console.log(solution(input));
```
