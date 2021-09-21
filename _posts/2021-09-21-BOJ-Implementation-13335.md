---
layout: post
title: "BOJ[13335] - 트럭 by JavaScript"
date: 2021-09-21 12:00:00 +0900
categories: BOJ(Implementation)
---

# 트럭

## 문제

- [백준 13335번 - 트럭](https://www.acmicpc.net/problem/13335)

## 언어

- JavaScript

## 순서도

1. 1 초씩 늘려가며,
2. 다리가 비어있지 않고, 맨 앞의 트럭이 다리를 진입 후 시간이 충분히 지났다면 트럭 이동시켜주기
3. 트럭이 남아있고, 트럭이 다리를 올라갈 수 있다면, 다리에 트럭 진입 시키기
4. 모든 트럭이 다리를 다 지나가면, 소요 시간 출력하기

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 강을 가로지르는 하나의 차선으로 된 다리가 하나 있습니다.
  - 이 다리를 n 개의 트럭이 건너가려고 한다.
  - 트럭의 순서는 바꿀 수 없으며, 트럭의 무게는 서로 같지 않을 수 있습니다.
  - 다리 위에는 단지 w 대의 트럭만 동시에 올라갈 수 있습니다.
  - 다리의 길이는 w 단위 길이, 각 트럭은 하나의 단위 시간에 하나의 단위 길이 만큼만 이동할 수 있습니다.
  - 동시에 다리 위에 올라가 있는 트럭들의 무게의 합은 다리의 최대 하중인 L 보다 작거나 같아야 합니다.
  - 참고로 다리 위에 완전히 올라가지 못한 트럭의 무게는 다리 위의 트럭들의 무게의 합을 계산할 때 포함하지 않습니다.

## 문제 풀이 step 2

- 위의 정보를 기반으로 모든 트럭이 다리를 지나가는데 걸리는 시간을 출력하는 문제입니다.
- 트럭의 순서를 바꿀 수 없기 때문에 선입선출 형식의 자료구조인 큐를 이용해서 다리를 구현합니다.
- 1 초씩 늘려가며, 트럭의 이동을 구현합니다.
  - 우선 만약 다리의 맨 앞의 트럭이 다리를 지나갈 만큼의 시간이 소요되었다면, 트럭을 제거해줍니다.
  - 만약 아직 다리를 지나가지 않은 트럭이 존재하고, 그 트럭이 다리위에 올라와도 무게가 충분하면 트럭을 올려줍니다.
  - 이 과정을 모든 트럭이 다리를 지나갈 때까지 반복합니다.
- 최종적으로 소요 시간을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 오늘은 추석 당일인 화요일입니다. 생각이 많은 하루입니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

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

	peek() {
		return this.bucket[this.front + 1];
	}
}

const solution = (input) => {
	const [n, w, l] = input[0].split(" ").map(Number);
	const trucks = input[1].split(" ").map(Number);

	// 다리위의 무게 정보, 각 버스의 번호, 시간
	let weight = 0;
	const bridge = new Queue();
	let index = 0;
	let time = 0;
	while (true) {
		// 1 초 소요
		time += 1;

		// 다리가 비어있지 않다면,
		if (!bridge.isEmpty()) {
			// 맨 앞의 트럭이
			const [firstIndex, firstTime] = bridge.peek();

			// 다리를 지났다면,
			if (time - firstTime === w) {
				bridge.dequeue();
				weight -= trucks[firstIndex];
			}
		}

		// 다리를 지나지 않은 트럭이 있고, 트럭이 다리 위에 올라와도 무게가 충분하면,
		if (index < n && weight + trucks[index] <= l) {
			weight += trucks[index];
			bridge.enqueue([index, time]);
			index += 1;
		}

		// 모든 트럭이 다리를 지났다면,
		if (bridge.isEmpty()) break;
	}

	// 시간 출력
	return time;
};

console.log(solution(input));
```
