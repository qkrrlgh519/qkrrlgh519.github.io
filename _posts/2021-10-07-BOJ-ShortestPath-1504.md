---
layout: post
title: "BOJ[1504] - 특정한 최단 경로 by JavaScript"
date: 2021-10-07 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 특정한 최단 경로

## 문제

- [백준 1504번 - 특정한 최단 경로](https://www.acmicpc.net/problem/1504)

## 언어

- JavaScript

## 순서도

1. 최소 힙 구현
2. 다익스트라 알고리즘 함수 구현
3. 1 에서 v1 을 통해 v2 를 지나 n 으로 도달하는 경로와 1 에서 v2 를 통해 v1 을 지나 n 으로 도달하는 경로 생성
4. 각 경로의 부분 경로에 대해서 다익스트라 알고리즘을 적용해서 각 경로의 최단 경로 구하기
5. 두 개의 최단 경로 중에서 최소값 찾기

## 문제 풀이 step 1

- 방향성이 없는 그래프가 주어집니다. 세준이는 1 번 정점에서 N 번 정점으로 최단 거리로 이동하려고 합니다.
- 또한 세준이는 두 가지 조건을 만족하면서 이동하는 특정한 최단 경로를 구하고 싶은데, 그것은 바로 임의로 주어진 두 정점은 반드시 통과해야 한다는 것입니다.
- 세준이는 한번 이동했던 정점은 물론, 한번 이동했던 간선도 다시 이동할 수 있습니다. 하지만 반드시 최단 경로로 이동해야 합니다.
- 1 번 정점에서 N 번 정점으로 이동할 때, 주어진 두 정점을 반드시 거치는 최단 경로를 구하는 문제입니다.

## 문제 풀이 step 2

- 다익스트라 알고리즘을 이용하면 쉽게 풀 수 있습니다.
- 우선, 최소 힙을 구현하고, 주어진 입력에 대해서 인접리스트 형식의 양방향 그래프를 생성합니다.
- 1 번 정점에서 N 번 정점으로 이동할 때, 주어진 두 정점을 반드시 거치는 최단 경로의 경우의 수는 2 가지 입니다.
  - 1 번 정점에서 첫 번째 정점을 통해 두 번째 정점을 지나 N 번 정점에 도달하는 방법
  - 1 번 정점에서 두 번째 정점을 통해 첫 번째 정점을 지나 N 번 정점에 도달하는 방법
- 이 두 가지의 경로에 대해서 최단 경로를 구하면 되겠습니다.
  - 이 때, 다익스트라 알고리즘을 이용해서 각 부분 경로의 최단 경로를 구함으로써 전체적인 최단 경로를 구합니다.
  - 예를 들어, 첫 번째 경우
    - 1 번 정점에서 첫 번째 정점으로의 최단 경로
    - 첫 번째 정점에서 두 번째 정점으로의 최단 경로
    - 두 번째 정점에서 N 번 정점으로의 최단 경로
    - 위의 3 개의 부분 경로의 최단 경로를 통해서 구할 수 있습니다.
- 최단 경로를 구하는 도중에 그런 경로가 없을 경우 -1 을 출력하고, 두 경로 모두 최단 경로가 구해지면, 두 개 중에서 최소인 최단 경로를 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const INF = Number.MAX_SAFE_INTEGER;

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

// 다익스트라 알고리즘 구현
const dijkstra = (n, graph, start) => {
	const minHeap = new MinHeap();
	const distance = Array(n + 1).fill(INF);

	minHeap.insert([0, start]);
	distance[start] = 0;

	while (!minHeap.isEmpty()) {
		const [fromCost, from] = minHeap.delete();

		if (distance[from] < fromCost) continue;

		for (let i = 0; i < graph[from].length; i++) {
			const to = graph[from][i][0];
			const toCost = graph[from][i][1] + fromCost;

			if (distance[to] > toCost) {
				distance[to] = toCost;
				minHeap.insert([toCost, to]);
			}
		}
	}

	return distance;
};

// 각 부분 경로에 다익스트라 알고리즘을 적용해 전체 경로의 최단 경로를 구하는 함수
const getWholeDistance = (n, graph, route) => {
	let wholeDistance = 0;

	for (let i = 0; i < route.length - 1; i++) {
		// 각 부분 경로에 다익스트라 알고리즘 적용
		const [from, to] = [route[i], route[i + 1]];

		const distance = dijkstra(n, graph, from);
		if (distance[to] === INF) return -1;
		wholeDistance += distance[to];
	}

	return wholeDistance;
};

const solution = (input) => {
	const [n, e] = input[0].split(" ").map(Number);

	// 양방향 그래프 생성
	const graph = Array.from({length: n + 1}, () => []);
	for (let i = 1; i < e + 1; i++) {
		const [from, to, cost] = input[i].split(" ").map(Number);

		graph[from].push([to, cost]);
		graph[to].push([from, cost]);
	}

	// 두 개의 경유 정점
	const [via1, via2] = input[e + 1].split(" ").map(Number);

	// 첫 번재 경로: 1 => v1 => v2 => n
	let route = [1, via1, via2, n];
	const firstWay = getWholeDistance(n, graph, route);
	if (firstWay === -1) return -1;

	// 두 번째 경로: 1 => v2 => v1 => n
	route = [1, via2, via1, n];
	const secondWay = getWholeDistance(n, graph, route);
	if (secondWay === -1) return -1;

	// 최소값 return
	return Math.min(firstWay, secondWay);
};

console.log(solution(input));
```
