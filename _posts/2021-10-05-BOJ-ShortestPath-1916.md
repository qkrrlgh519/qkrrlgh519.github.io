---
layout: post
title: "BOJ[1916] - 최소비용 구하기 by JavaScript"
date: 2021-10-05 12:00:00 +0900
categories: BOJ(ShortestPath)
---

# 최소비용 구하기

## 문제

- [백준 1916번 - 최소비용 구하기](https://www.acmicpc.net/problem/1916)

## 언어

- JavaScript

## 순서도

1. 최소 힙 구현
2. 방향 그래프 생성
3. 다익스트라 알고리즘 수행

## 문제 풀이 step 1

- N 개의 도시와 한 도시에서 출발하여 다른 도시에 도착하는 M 개의 버스가 있습니다.
- 이 때, A 번째 도시에서 B 번째 도시까지 가는데 드는 버스 비용의 최솟값을 구하는 문제입니다.
- 다익스트라 알고리즘을 이용하면 쉽게 풀 수 있습니다.
- 우선, 최소 힙을 구현하고, 주어진 입력에 대해서 인접리스트 형식의 방향 그래프를 생성합니다.
- 그리고 시작점으로부터 각 정점으로의 최소 비용을 기록하는 distance 배열을 생성합니다.
- 먼저, 시작점의 최소 비용을 0 으로 갱신하고 최소 힙에 [최소 비용, 시작점] 정보를 넣어줍니다.
- 최소 힙이 텅 빌때까지 다익스트라 알고리즘을 수행합니다.
  - 먼저, 최소 힙에서 원소를 하나 뺍니다.
  - 빼낸 원소의 최소 비용이 distance 배열에 기록된 값보다 크다면 무시합니다.
  - distance 배열에 기록된 값보다 작다면 연결된 간선들을 다 검사하며 distance 배열을 갱신하고 최소 힙에 넣어줍니다.
- distance 배열에서 도착점까지의 비용을 찾아서 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

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
	const INF = 1000000000;

	const n = Number(input[0]);
	const m = Number(input[1]);

	// 인접리스트 형식의 방향 그래프 생성
	const graph = Array.from({length: n + 1}, () => []);
	for (let i = 2; i < m + 2; i++) {
		const [from, to, cost] = input[i].split(" ").map(Number);

		graph[from].push([to, cost]);
	}

	// 시작점과 도착점
	const [start, end] = input[m + 2].split(" ").map(Number);
	const minHeap = new MinHeap();
	const distance = Array(n + 1).fill(INF);

	// 시작점 정보 입력
	minHeap.insert([0, start]);
	distance[start] = 0;

	// 최소힙이 텅 빌때까지
	while (!minHeap.isEmpty()) {
		const [fromCost, from] = minHeap.delete();

		// 최소 힙에서 뺀 원소의 최소 비용이 더 크다면 무시
		if (distance[from] < fromCost) continue;

		// 연결된 간선들을 모두 확인하며 최소 비용 갱신
		for (let i = 0; i < graph[from].length; i++) {
			const to = graph[from][i][0];
			const toCost = fromCost + graph[from][i][1];

			if (distance[to] > toCost) {
				distance[to] = toCost;
				minHeap.insert([toCost, to]);
			}
		}
	}

	// 도착점까지의 최소 비용 출력
	return distance[end];
};

console.log(solution(input));
```
