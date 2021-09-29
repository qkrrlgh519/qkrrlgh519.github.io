---
layout: post
title: "BOJ[15903] - 카드 합체 놀이 by JavaScript"
date: 2021-09-29 12:00:00 +0900
categories: BOJ(DataStructure)
---

# 카드 합체 놀이

## 문제

- [백준 15903번 - 카드 합체 놀이](https://www.acmicpc.net/problem/15903)

## 언어

- JavaScript

## 순서도

1. 주어진 n 개의 자연수 모두 우선순위 큐에 넣기
2. 우선순위 큐에서 두 개의 값을 뺀 후 더하고 다시 넣는 작업을 m 번 반복하기
3. 우선순위 큐에 들어있는 모든 자연수를 더하기

## 문제 풀이 step 1

- 석환이가 카드 합체 놀이를 하려고 합니다.
- 카드 합체 놀이란 자연수가 쓰여진 카드 n 장 중에서 임의의 두 장의 카드를 더하고, 더한 값을 두 장의 카드에 덮어 쓰는 놀이입니다.
- 이 카드 합체를 총 m 번 하면 놀이가 끝납니다. m 번의 합체를 모두 끝낸 뒤, n 장의 카드에 쓰여있는 수를 모두 더한 값이 이 놀이의 점수가 됩니다.
- 점수를 가장 작게 만드는 것이 놀이의 목표입니다.

## 문제 풀이 step 2

- 위와 같이 카드 합체 놀이에 대한 설명이 있을 때, 가장 작은 점수를 계산하는 문제입니다.
- 기존에 있는 두 개의 값이 서로 합한 값으로 바뀐다면, 전체 합을 작게 만들기 위해서는 두 개의 값이 가장 작은 값이어야 할 것입니다.
- 따라서 우선순위 큐를 하나 생성합니다. 그리고 우선순위 큐에 주어진 n 개의 자연수를 모두 넣어줍니다.
- 그리고 우선순위 큐에서 가장 작은 값 2 개를 추출합니다. 그리고 2 개의 값을 더하고, 그 합을 우선순위 큐에 2 번 넣어줍니다.
- 위 과정을 m 번 반복한 후에 우선순위 큐에 들어있는 모든 수를 더하면 정답입니다.
- 단, 문제에서 수의 범위가 상당히 크기 때문에 BigInt 형으로 변환해서 풀어야 합니다.
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

		while (index !== 1 && item < this.bucket[parseInt(index / 2)]) {
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
				this.bucket[childIdx] > this.bucket[childIdx + 1]
			)
				childIdx += 1;

			if (lastItem <= this.bucket[childIdx]) break;

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
	let [n, m] = input[0].split(" ").map(Number);
	const arr = input[1].split(" ").map(BigInt);

	// 최소 힙에 n 개의 자연수 모두 넣기
	const minHeap = new MinHeap();
	for (let i = 0; i < n; i++) {
		minHeap.insert(arr[i]);
	}

	while (m-- > 0) {
		// 2 개의 값을 가져오고,
		const a = minHeap.delete();
		const b = minHeap.delete();

		// 더하고
		const sum = a + b;

		// 그 합을 2 번 최소 힙에 넣기
		minHeap.insert(sum);
		minHeap.insert(sum);
	}

	// 최소 힙에 들어있는 모든 수를 더하면 정답
	let res = 0n;
	while (!minHeap.isEmpty()) {
		res += minHeap.delete();
	}

	return res.toString();
};

console.log(solution(input));
```
