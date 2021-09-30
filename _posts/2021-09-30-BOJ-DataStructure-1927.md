---
layout: post
title: "BOJ[1927] - 최소 힙 by JavaScript"
date: 2021-09-30 12:00:00 +0900
categories: BOJ(DataStructure)
---

# 최소 힙

## 문제

- [백준 1927번 - 최소 힙](https://www.acmicpc.net/problem/1927)

## 언어

- JavaScript

## 순서도

1. 최소 힙 구현

## 문제 풀이 step 1

- 최소 힙을 구현하는 문제입니다.
  - 자세한 설명은 [Priority Queue, Heap 포스트](https://qkrrlgh519.github.io/datastructure/2021/06/18/Datastructure-PriorityQueue-Heap.html) 을 참고하시면 좋을 것 같습니다.
- 배열에 기반해서 insert 함수와 delete 함수 그리고 isEmpty 함수를 구현해주면 됩니다.
  - 구현을 쉽게 하기 위해서 0 번 인덱스는 사용하지 않습니다. 따라서 null 값을 넣어줬습니다.
  - 이렇게 하면,
    - `부모 index x 2 = 왼쪽 자식 index`,
    - `부모 index x 2 + 1 = 오른쪽 자식 index`,
    - `부모 index = (INT) (자식 index / 2)`
  - 성립합니다.
- insert 함수
  - 우선, 배열의 가장 마지막에 값을 넣어줍니다. 그리고 부모의 값과 비교하며 적절한 위치를 찾은 다음에 넣어준 값을 고정합니다.
  - 최소 힙의 경우는 작은 값이 위에 있어야 하므로, 부모의 값보다 insert 한 값이 더 작다면 위로 올리고, 부모의 값보다 insert 한 값이 더 크다면 위치가 고정됩니다.
- delete 함수
  - 우선 배열의 가장 앞에 있는 값을 저장하고, 후에 return 합니다. 왜냐하면 배열의 가장 앞에 있는 값이 가장 작은 값이기 때문입니다.
  - 그리고 배열의 가장 마지막에 있는 값을 맨 위로 가져옵니다. 그리고 자식들의 값과 비교하며 적절한 위치를 찾은 다음에 값을 고정합니다.
  - 최소 힙의 경우는 작은 값이 위에 있어야 하므로, 자식의 값보다 가져온 값이 더 크다면 아래로 내리고, 자식의 값보다 가져온 값이 더 작다면 위치가 고정됩니다.
- isEmpty 함수
  - 구현을 쉽게 하기 위해서 배열의 맨 앞에 null 값을 넣어줬으므로, 배열의 길이가 1 인지의 비교를 통해서 구현합니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

class MinHeap {
	constructor() {
		this.bucket = [null];
	}

	insert(item) {
		// 배열의 가장 마지막에 item 넣기
		let index = this.bucket.length;

		// index 가 1 이 아니면서, 부모의 값보다 작을 때까지
		while (index !== 1 && item < this.bucket[parseInt(index / 2)]) {
			// 부모의 값을 끌어 내리기
			this.bucket[index] = this.bucket[parseInt(index / 2)];
			index = parseInt(index / 2);
		}

		// 반복이 끝나면 index 의 위치에 item 넣기
		this.bucket[index] = item;
	}

	delete() {
		if (this.isEmpty()) return;

		let parentIdx = 1;
		let childIdx = 2;
		let item = this.bucket[1];
		let lastItem = this.bucket.pop();

		while (childIdx < this.bucket.length) {
			// 왼쪽 자식과 오른쪽 자식 중에서 더 작은 값 찾기
			if (
				childIdx + 1 < this.bucket.length &&
				this.bucket[childIdx] > this.bucket[childIdx + 1]
			) {
				childIdx += 1;
			}

			// 가져온 값이 자식의 값보다 작다면 break;
			if (lastItem <= this.bucket[childIdx]) break;

			// 가져온 값이 자식의 값보다 크다면 자식의 값 끌어올리기
			this.bucket[parentIdx] = this.bucket[childIdx];

			// index 갱신
			parentIdx = childIdx;
			childIdx *= 2;
		}

		// 힙에 값이 남아있다면 부모 index 에 마지막에서 가져온 item 고정
		if (!this.isEmpty()) this.bucket[parentIdx] = lastItem;

		// 배열에서 가장 앞에 있던 값 return
		return item;
	}

	isEmpty() {
		return this.bucket.length === 1;
	}
}

const solution = (input) => {
	const n = Number(input[0]);

	let res = "";
	const minHeap = new MinHeap();
	for (let i = 1; i < n + 1; i++) {
		const x = Number(input[i]);

		if (x === 0) {
			if (!minHeap.isEmpty()) {
				res += minHeap.delete() + "\n";
			} else {
				res += 0 + "\n";
			}

			continue;
		}

		minHeap.insert(x);
	}

	return res;
};

console.log(solution(input));
```
