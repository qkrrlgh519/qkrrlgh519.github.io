---
layout: post
title: "BOJ[16236] - 아기 상어 by JavaScript"
date: 2021-06-29 13:00:00 +0900
categories: BOJ(Implementation)
---

# 아기 상어

## 문제

- [백준 16236번 - 아기 상어](https://www.acmicpc.net/problem/16236)

## 언어

- JavaScript

## 순서도

1. 상어 관련한 변수 생성 (상어의 크기, 상어가 먹은 물고기 수, 상어의 위치) 및 지도에서 상어의 위치 찾기
2. BFS 탐색을 통해서 현재 상어의 위치에서 가장 가까운 먹을 수 있는 물고기의 위치 찾기
3. 그 물고기의 위치로 이동해서 물고기를 먹고, 상어의 위치를 갱신하고 소요 시간을 계산
4. 2 번과 3 번을 반복하며, 상어가 먹을 수 있는 물고기가 없을 때까지 반복
5. 여태까지의 소요 시간 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 주어진 공간에서 상어가 문제에서 주어진 조건대로 움직이며 물고기를 먹다가 더이상 먹을 수 없는 상황이 되면 그 때의 소요 시간을 출력하는 문제입니다.
- 상어의 특징은
  - 초기 크기는 2 입니다.
  - 상하좌우로 이동하는데 1 초 소요됩니다.
  - 자신의 크기보다 작은 물고기만 먹을 수 있습니다.
  - 자신의 크기와 같은 물고기는 지나갈 수만 있습니다.
  - 자신의 크기보다 큰 물고기는 지나갈 수조차 없습니다.
  - 더 이상 먹을 수 있는 물고기가 없을 때까지 이동합니다.
  - 물고기가 있는 칸으로 이동할 때는 최소 거리로 이동합니다.
  - 거리가 가까운 물고기가 많다면 가장 위에 있는 물고기를 먹고, 그러한 물고기가 여러마리라면 가장 왼쪽에 있는 물고기를 먹습니다.
  - 자신의 크기와 같은 수의 물고기를 먹을 때마다 크기가 1 증가합니다.
- 위의 특징과 BFS 탐색을 통해서 푸는 문제입니다.

## 문제 풀이 step 2

- 우선 상어에 관해서 필요한 변수를 정의합니다.
  - sharkSize : 상어의 크기
  - sharkFood : 상어가 먹은 물고기 수
  - sharkPos : 상어의 현재 위치
- 그리고 주어진 공간에서 상어의 초기 위치를 찾아줍니다. (초기 위치는 공간내에서 값이 9 인 곳을 찾으면 됩니다.)
- BFS 탐색을 수행합니다.
  - 탐색을 통해서 현재 상어의 위치에서 자신보다 크기가 작은 물고기의 위치까지의 거리를 모두 구합니다.
  - 구한 거리 중에서 최단 거리를 구합니다. 이때, 가장 위에 있으며 가장 왼쪽에 있는 물고기를 최우선으로 선택합니다.
- BFS 탐색을 통해서 구한 가장 가까운 물고기의 위치로 이동해서 물고기를 먹습니다.
  - 한 칸 이동하는데 1 초 소요되니까, 거리만큼 소요 시간을 갱신합니다.
  - 상어가 먹은 물고기의 수를 갱신하고, 자신의 크기만큼 물고기를 먹었다면 물고기의 크기도 갱신해줍니다.
- BFS 탐색을 수행하다가 더 이상 먹을 수 있는 물고기가 없을 때, 여태까지 계산한 소요 시간을 출력하면 정답이 됩니다.
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

// 간단한 큐 구현, 배열을 사용해도 무방합니다.
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

// 다음으로 가려는 곳이 공간을 벗어나는지 검사하는 함수
const isPossibleRoute = (n, to) =>
	0 <= to.x && to.x < n && 0 <= to.y && to.y < n;

// 가장 위이며, 가장 왼쪽인 최단 거리를 찾는 함수
const getClosest = (n, arr, distance, sharkSize) => {
	let min = 1000;
	let dot = null;

	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			if (distance[i][j] === -1) continue;

			if (0 < arr[i][j] && arr[i][j] < sharkSize) {
				if (min > distance[i][j]) {
					min = distance[i][j];
					dot = new Dot(i, j);
				}
			}
		}
	}

	return [min, dot];
};

const BFS = (n, arr, sharkPos, sharkSize) => {
	// 상어는 상하좌우로 이동합니다.
	const cx = [-1, 1, 0, 0];
	const cy = [0, 0, -1, 1];

	// 현재 상어의 위치에서 각 물고기까지의 거리를 구하기 위한 2 차원 배열
	const distance = Array.from({length: n}, () =>
		Array.from({length: n}, () => -1)
	);
	const queue = new Queue();
	distance[sharkPos.x][sharkPos.y] = 0;
	queue.enqueue(sharkPos);

	while (!queue.isEmpty()) {
		const from = queue.dequeue();

		for (let i = 0; i < 4; i++) {
			for (let j = 0; j < 4; j++) {
				const to = new Dot(from.x + cx[i], from.y + cy[i]);
				if (!isPossibleRoute(n, to) || distance[to.x][to.y] !== -1) continue;
				if (arr[to.x][to.y] > sharkSize) continue;

				// 거리 갱신
				distance[to.x][to.y] = distance[from.x][from.y] + 1;
				queue.enqueue(to);
			}
		}
	}

	// 위의 탐색을 통해서 나온 distance 배열을 이용해서 최단 거리 구해서 return
	return getClosest(n, arr, distance, sharkSize);
};

const solution = (input) => {
	const n = Number(input[0]);
	const arr = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	// 상어 관련 변수 생성
	let sharkSize = 2;
	let sharkFood = 0;
	let sharkPos = null;
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			if (arr[i][j] === 9) {
				sharkPos = new Dot(i, j);
				arr[i][j] = 0;
				break;
			}
		}
	}

	let time = 0;
	while (true) {
		// BFS 탐색을 통해서 최단 거리와 좌표 구하기
		const [distance, to] = BFS(n, arr, sharkPos, sharkSize);

		// BFS 탐색시 먹을 수 있는 물고기가 없으면 distance 가 1000 을 return 하도록 구현되어 있습니다.
		if (distance === 1000) break;

		// 물고기를 먹고, 시간을 갱신하고, 상어가 먹은 물고기 수 갱신하고, 상어의 크기 갱신하기
		arr[to.x][to.y] = 0;
		time += distance;
		sharkPos = to;
		sharkFood += 1;
		// 자신의 크기만큼 물고기를 먹었다면, 상어의 크기를 갱신
		if (sharkSize === sharkFood) {
			sharkFood -= sharkSize;
			sharkSize += 1;
		}
	}

	// 여태까지의 소요 시간 출력
	return time;
};

console.log(solution(input));
```
