---
layout: post
title: "이것이 코딩테스트다 - [ShortestPath] 전보 by JavaScript"
date: 2021-12-16 12:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 전보

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 한 도시에서 갈 수 있는 모든 도시에 메시지를 보내려고 합니다. 그리고 최대한 빠르게 메시지를 보내고 싶습니다.
- 이 때, 메시지를 보낼 수 있는 도시의 개수와 모든 도시가 메시지를 받는 데 까지 걸리는 시간을 구하는 문제입니다.
- 다익스트라 알고리즘을 이용해서, 메시지를 보낼 도시로부터 다른 모든 도시와의 최단 거리를 구하고, 정답을 구하면 되겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");
// const input = `3 2 1
// 1 2 4
// 1 3 2`.split("\n");

// 우선 순위 큐 구현
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

		let item = this.bucket[1];
		let lastItem = this.bucket.pop();

		let parentIdx = 1;
		let childIdx = 2;

		while (childIdx < this.bucket.length) {
			if (
				childIdx + 1 < this.bucket.length &&
				this.bucket[childIdx][0] > this.bucket[childIdx + 1][0]
			)
				childIdx += 1;

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

const INF = Math.floor(Number.MAX_SAFE_INTEGER / 2);

const solution = (input) => {
	const [n, m, c] = input[0].split(" ").map(Number);
	const graph = Array.from(Array(n), () => []);

	// 인접리스트 형식의 그래프 생성
	for (let i = 1; i < m + 1; i++) {
		const [x, y, c] = input[i].split(" ").map(Number);

		graph[x - 1].push([y - 1, c]);
	}

	// 우선순위 큐와 distance 배열 생성
	const pq = new PriorityQueue();
	const distance = Array(n).fill(INF);

	// 시작점은 가중치 0 으로 해서 우선순위 큐와 distance 배열에 초기화
	pq.enqueue([0, c - 1]);
	distance[c - 1] = 0;

	// 우선순위 큐가 빌 때까지
	while (!pq.isEmpty()) {
		const [weight, to] = pq.dequeue();

		// 이미 저장된 경로보다 가중치가 더 큰 경로는 제외
		if (weight > distance[to]) continue;

		// to 정점을 통해서 갈 수 있는 모든 경로에 대해서
		for (let i = 0; i < graph[to].length; i++) {
			const next = graph[to][i][0];
			const nextWeight = graph[to][i][1] + weight;

			// 경로가 더 짧다면 갱신해주고 우선순위 큐에 넣기
			if (distance[next] > nextWeight) {
				distance[next] = nextWeight;
				pq.enqueue([nextWeight, next]);
			}
		}
	}

	// 메시지를 전달할 수 있는 도시의 개수와 총 걸리는 시간 찾기
	let cnt = 0;
	let max = 0;
	for (let i = 0; i < n; i++) {
		if (i === c - 1) continue;
		if (distance[i] === INF) continue;

		cnt += 1;
		max = Math.max(max, distance[i]);
	}

	return cnt + " " + max;
};

console.log(solution(input));
```
