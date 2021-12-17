---
layout: post
title: "BOJ[11779] - 최소비용 구하기 2 by JavaScript"
date: 2021-12-17 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 최소비용 구하기 2

## 문제

- [백준 11779번 - 최소비용 구하기 2](https://www.acmicpc.net/problem/11779)

## 언어

- JavaScript

## 순서도

1. 우선순위큐 구현
2. 방향 그래프 생성
3. 다익스트라 알고리즘 수행 (이와 동시에 각 경로가 어떤 경유지를 거쳤는지 기록)

## 문제 풀이 step 1

- n 개의 도시가 있습니다. 그리고 한 도시에서 출발하여 다른 도시에 도착하는 m 개의 버스가 있습니다.
- 우리는 A 번째 도시에서 B 번째 도시까지 가는데 드는 버스 비용을 최소화 시키려고 합니다.
- A 번째 도시에서 B 번째 도시까지 가는데 드는 최소 비용과 경로를 출력하는 문제입니다.
- 항상 시작점에서 도착점으로의 경로가 존재합니다.

## 문제 풀이 step 2

- 다익스트라 알고리즘을 적용하면 쉽게 풀 수 있습니다.
- 우선, 우선순위큐를 구현하고, 주어진 입력에 대해서 인접리스트 형식의 방향 그래프를 생성합니다.
- 그리고 시작점으로부터 각 정점으로의 최소 비용을 기록하는 distance 배열을 생성합니다.
- **이에 추가로 각 경로가 어떤 경유지를 통해서 왔는지를 기록하는 waypoint 배열을 생성**합니다.
- 그리고 로직은 다익스트라 알고리즘과 동일합니다.
- 먼저, 시작점의 최소 비용을 0 으로 갱신하고 우선순위큐에 [최소 비용, 시작점] 정보를 넣어줍니다.
- 우선순위큐가 텅 빌때까지 다익스트라 알고리즘을 수행합니다.
  - 먼저, 우선순위큐에서 원소를 하나 추출합니다.
  - 빼낸 원소의 최소 비용이 distance 배열에 기록된 값보다 크다면 무시합니다.
  - distance 배열에 기록된 값보다 작다면 연결된 간선들을 다 검사하며 distance 배열을 갱신하고 최소 힙에 넣어줍니다.
  - **이에 추가로 distance 배열을 갱신할 때, waypoint 배열에 경유한 정점을 기록**해줍니다.
- distance 배열에서 도착점까지의 비용을 출력합니다.
- 그리고 **waypoint 배열을 거슬러 올라가며, 어떤 정점을 통해서 도착점에 도달했는지, 이동 경로를 구해서 출력**하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 기존의 다익스트라 알고리즘에 경유지를 기록하는 로직만 추가하면 되는 문제입니다.
- 이 방법을 쉽게 떠올릴 수 있게 가르쳐주셨던 교수님 감사합니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 우선순위큐 구현
class PriorityQueue {
	constructor() {
		this.bucket = [null];
	}

	enqueue(item) {
		let index = this.bucket.length;

		while (index !== 1 && item[0] < this.bucket[parseInt(index / 2)][0]) {
			this.bucket[index] = this.bucket[parseInt(index / 2)];
			index = parseInt(index / 2);
		}

		this.bucket[index] = item;
	}

	dequeue() {
		if (this.isEmpty()) return;

		const item = this.bucket[1];
		const lastItem = this.bucket.pop();
		let [parentIdx, childIdx] = [1, 2];

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

const INF = Math.floor(Number.MAX_SAFE_INTEGER);

const solution = (input) => {
	const n = Number(input[0]);
	const m = Number(input[1]);

	// 인접리스트 형식의 그래프 생성
	const graph = Array.from(Array(n + 1), () => []);

	for (let i = 2; i < m + 2; i++) {
		const [from, to, cost] = input[i].split(" ").map(Number);

		graph[from].push([to, cost]);
	}

	// 시작점과 도착점
	const [start, end] = input[m + 2].split(" ").map(Number);

	const pq = new PriorityQueue();
	const distance = Array(n + 1).fill(INF);

	// 각 정점에 도달할 때 어떤 정점을 경유했는지 기록하는 배열
	const waypoint = Array(n + 1).fill(-1);

	pq.enqueue([0, start]);
	distance[start] = 0;

	while (!pq.isEmpty()) {
		const [cost, to] = pq.dequeue();

		// 새로운 경로의 비용이 distance 배열에 저장된 값보다 작다면 무시
		if (distance[to] < cost) continue;

		for (let i = 0; i < graph[to].length; i++) {
			const next = graph[to][i][0];
			const nextCost = cost + graph[to][i][1];

			if (nextCost < distance[next]) {
				distance[next] = nextCost;
				pq.enqueue([nextCost, next]);
				// distance 배열 갱신시 경유지 기록
				waypoint[next] = to;
			}
		}
	}

	const route = [end];

	// 도착점으로부터 시작점까지 거슬러 올라가면서 이동 경로 찾기
	let index = end;
	while (index !== start) {
		route.push(waypoint[index]);
		index = waypoint[index];
	}

	return `${distance[end]}\n${route.length}\n${route.reverse().join(" ")}`;
};

console.log(solution(input));
```
