---
layout: post
title: "BOJ[1753] - 최단경로 by JavaScript"
date: 2021-10-04 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 최단경로

## 문제

- [백준 1753번 - 최단경로](https://www.acmicpc.net/problem/1753)

## 언어

- JavaScript

## 순서도

1. 최소 힙 구현
2. 방향 그래프 생성
3. 다익스트라 알고리즘 수행

## 문제 풀이 step 1

- 방향그래프가 주어지면 주어진 시작점에서 다른 모든 정점으로의 최단 경로를 구하는 문제입니다.
- 다익스트라 알고리즘을 이용하면 쉽게 풀 수 있습니다.
- 우선, 최소 힙을 구현하고, 주어진 입력에 대해서 인접리스트 형식의 방향 그래프를 생성합니다.
- 그리고 시작점으로부터 각 정점으로의 최단 경로를 기록하는 distance 배열을 생성합니다.
- 먼저, 시작점의 최단 경로를 0 으로 갱신하고 최소 힙에 [최단 경로, 시작점] 정보를 넣어줍니다.
- 최소 힙이 텅 빌때까지 다익스트라 알고리즘을 수행합니다.
  - 먼저, 최소 힙에서 원소를 하나 뺍니다.
  - 빼낸 원소의 최단 경로가 distance 배열에 기록된 값보다 크다면 무시합니다.
  - distance 배열에 기록된 값보다 작다면 연결된 간선들을 다 검사하며 distance 배열을 갱신하고 최소 힙에 넣어줍니다.
- 위 과정을 통해 값이 다 채워진 distance 배열을 적절하게 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 다익스트라 알고리즘을 공부하고, 처음 풀어보는 문제였습니다.
- 다익스트라 알고리즘 내에서 distance 배열에 기록된 값의 이상인 최단 경로만 무시하려고 했는데 그러면 시작 정점도 무시되었습니다.
- 그래서 distnace 배열에 기록된 값을 초과하는 최단 경로만 무시하는 방식으로 풀었습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 문제에서 주어진 값의 제한을 생각해서 넉넉하게 INF 값 설정
const INF = 400000;

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

const solution = (input) => {
	const [v, e] = input[0].split(" ").map(Number);
	const k = Number(input[1]);

	// 인접리스트 형식의 그래프 생성 (방향 그래프)
	const graph = Array.from({length: v + 1}, () => []);
	for (let i = 2; i < 2 + e; i++) {
		const [u, v, w] = input[i].split(" ").map(Number);

		// 한 방향의 데이터만 삽입
		graph[u].push([v, w]);
	}

	const minHeap = new MinHeap();
	const distance = Array.from({length: v + 1}, () => INF);

	// 시작점에 대한 정보 기록
	distance[k] = 0;
	minHeap.insert([0, k]);

	// 다익스트라 알고리즘
	while (!minHeap.isEmpty()) {
		const [fromW, fromV] = minHeap.delete();

		// distance 배열에 기록된 값보다 큰 최단 경로는 무시
		if (distance[fromV] < fromW) continue;

		// 연결된 간선을 모두 확인해보면서
		for (let i = 0; i < graph[fromV].length; i++) {
			const toV = graph[fromV][i][0];
			const toW = fromW + graph[fromV][i][1];

			// 더 작은 최단 경로가 있다면 distance 배열을 갱신하고, 최소 힙에 넣어주기
			if (distance[toV] > toW) {
				distance[toV] = toW;
				minHeap.insert([toW, toV]);
			}
		}
	}

	// 0 번 정점 제거
	distance.shift();

	return distance.map((v) => (v === INF ? "INF" : v)).join("\n");
};

console.log(solution(input));
```
