---
layout: post
title: "Datastructure - Priority Queue, Heap By Two Standards"
date: 2021-11-06 12:00:00 +0900
categories: Datastructure
---

# Priority Queue, Heap By Two Standards

## 0. 배경

- 우선 순위 큐에 관해서는 아래의 링크에 설명되어 있습니다.
  - [우선 순위 큐(Priority Queue) 포스트](https://qkrrlgh519.github.io/datastructure/2021/06/18/Datastructure-PriorityQueue-Heap.html)
- 이번에 다뤄볼 것은 **두 개의 기준을 가진 우선 순위 큐**입니다.
  - 어떤 기업의 코딩 테스트를 보다가 구현해볼 기회가 생겨서 포스트하게 되었습니다.

## 1. 두 개의 기준을 가진 우선 순위 큐 (Priority Queue)

- **배경**
  - 어떤 **버퍼**가 존재합니다. 이 버퍼는 처리해야할 작업들을 다루고 있습니다.
  - 이 버퍼는 **처리 시간이 짧은 작업들을 우선적으로 처리**합니다.
  - 이에 추가로 **처리 시간이 동일**하다면, **요청 시간이 먼저인 작업들을 우선적으로 처리**합니다.
  - **작업**들은 **[예상되는 처리 시간, 요청 시간]** 형태로 되어 있습니다.
- **해결**
  - **삽입 연산**
    - 새로운 요소를 힙의 마지막 노드에 이어서 삽입합니다.
    - 부모 노드와 비교합니다.
      - 새로운 노드의 첫 번째 요소가 부모 노드의 첫 번째 요소보다 작거나,
      - 새로운 노드의 첫 번째 요소와 부모 노드의 첫 번째 요소가 같고, 새로운 노드의 두 번째 요소가 부모 노드의 두 번째 요소보다 작으면,
      - 새로운 노드와 부모 노드를 교환합니다.
      - (부모 노드를 끌어 내리고, 새로운 노드는 올라가는 형태)
  - **삭제 연산**
    - 최상위 노드인 루트 노드를 삭제합니다.
    - 삭제된 루트 노드에 힙의 마지막 노드를 가져옵니다.
    - 새로운 루트 노드와 자식 노드들을 비교합니다.
      - 왼쪽 자식과 오른쪽 자식 노드들을 먼저 비교해서 교환하기에 더 적합한 노드를 결정합니다.
        - 왼쪽 자식과 오른쪽 자식 중 첫 번째 요소가 더 작은 노드로 결정합니다.
        - 왼쪽 자식과 오른쪽 자식의 첫 번째 요소가 같다면, 두 번째 요소가 더 작은 노드로 결정합니다.
      - 새로운 루트 노드와 결정된 자식 노드를 비교합니다.
        - 새로운 루트 노드의 첫 번째 요소가 자식 노드의 첫 번째 요소보다 크거나,
        - 새로운 루트 노드의 첫 번째 요소과 자식 노드의 첫 번째 요소가 같고, 새로운 루트 노드의 두 번째 요소가 자식 노드의 두 번째 요소보다 크다면,
        - 새로운 루트 노드와 자식 노드를 교환합니다.
        - (자식 노드를 끌어 올리고, 새로운 루트 노드는 내려가는 형태)
      - 위의 과정을 새로운 루트 노드의 첫 번째 요소가 더 작거나, 첫 번째 요소가 같고 두 번째 요소가 더 작은 상태가 될 때까지 반복합니다.
    - 위 과정을 통해서 새로운 루트 노드를 적합한 위치로 이동 시키고, 아까 삭제한 루트 노드를 반환합니다.

## 구현

- 언어 : JavaScript

```javascript
class MinHeap {
	constructor() {
		this.bucket = [null];
	}

	enqueue(item) {
		let index = this.bucket.length;

		while (index !== 1) {
			// 처리 시간이 더 짧거나
			// 처리 시간이 동일하고, 요청 시간이 더 먼저인 경우
			if (
				item[0] < this.bucket[parseInt(index / 2)][0] ||
				(item[0] === this.bucket[parseInt(index / 2)][0] &&
					item[1] < this.bucket[parseInt(index / 2)][1])
			) {
				this.bucket[index] = this.bucket[parseInt(index / 2)];
				index = parseInt(index / 2);
			} else {
				break;
			}
		}

		this.bucket[index] = item;
	}

	dequeue() {
		if (this.isEmpty()) return;

		let parentIdx = 1;
		let childIdx = 2;
		const item = this.bucket[1];
		const lastItem = this.bucket.pop();

		while (childIdx < this.bucket.length) {
			// 왼쪽과 오른쪽 요소 중에서
			// 오른쪽 요소의 처리 시간이 더 짧거나,
			// 두 요소의 처리 시간이 동일하고, 오른쪽 요소의 요청 시간이 더 먼저인 경우,
			if (
				childIdx + 1 < this.bucket.length &&
				(this.bucket[childIdx][0] > this.bucket[childIdx + 1][0] ||
					(this.bucket[childIdx][0] === this.bucket[childIdx + 1][0] &&
						this.bucket[childIdx][1] > this.bucket[childIdx + 1][1]))
			) {
				childIdx += 1;
			}

			// 마지막 요소의 처리 시간이 더 짧거나,
			// 마지막 요소와 처리 시간이 동일하고, 마지막 요소의 요청 시간이 더 먼저인 경우,
			if (
				lastItem[0] < this.bucket[childIdx][0] ||
				(lastItem[0] === this.bucket[childIdx][0] &&
					lastItem[1] < this.bucket[childIdx][1])
			)
				break;

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

// 실행
const jobs = [
	[3, 1],
	[4, 5],
	[2, 3],
	[4, 2],
	[0, 10],
];

const priorityQueue = new MinHeap();

for (let i = 0; i < jobs.length; i++) priorityQueue.enqueue(jobs[i]);

while (!priorityQueue.isEmpty()) console.log(priorityQueue.dequeue());

// 결과: [0, 10] [2, 3] [3, 1] [4, 2] [4, 5]
```
