---
layout: post
title: "BOJ[16235] - 나무 재테크 by JavaScript"
date: 2021-10-26 12:00:00 +0900
categories: BOJ(Implementation)
---

# 나무 재테크

## 문제

- [백준 16235번 - 나무 재테크](https://www.acmicpc.net/problem/16235)

## 언어

- JavaScript

## 순서도

1. N x N 크기의 배열 형태의 땅 구현 후, 나무 정보 입력
2. K 번 만큼 봄, 여름, 가을, 겨울 로직 적용시키기
3. 땅 배열을 전체 순회하며, 살아남은 나무의 개수 세기

## 문제 풀이 step 1

- N x N 크기의 땅이 있습니다. 땅은 1 x 1 크기의 칸으로 나누어져 있습니다.
  - 각각의 칸은 (r, c) 로 나타내며, r 은 행, c 는 열을 의미하며, r 과 c 는 1 부터 시작합니다.
- 로봇 S2D2 는 1 x 1 크기의 칸에 들어있는 양분을 조사해서 전송하고, 모든 칸에 대해서 조사합니다.
- 가장 처음에 양분은 모든 칸에 5 만큼씩 들어있습니다.
- 나무 재테크란 작은 묘목을 구매해 어느정도 키운 후 팔아서 수익을 얻는 재테크입니다.
- 나무 재테크로 큰 돈을 벌기 위해 M 개의 나무를 구매해 땅에 심었습니다.
- 같은 1 x 1 크기의 칸에 여러 개의 나무가 심어져 있을 수도 있습니다.
- 이 나무는 사계절을 보내며, 아래의 과정을 반복합니다.
- 봄에는 나무가 자신의 나이만큼 양분을 먹고, 나이가 1 증가합니다.
  - 각각의 나무는 나무가 있는 1 x 1 크기의 칸에 있는 양분만 먹을 수 있습니다.
  - 하나의 칸에 여러 개의 나무가 있다면, 나이가 어린 나무부터 양분을 먹습니다.
  - 만약, 땅에 양분이 부족해 자신의 나이만큼 양분을 먹을 수 없는 나무는 양분을 먹지 못하고 즉시 죽습니다.
- 여름에는 봄에 죽은 나무가 양분으로 변하게 됩니다.
  - 각각의 죽은 나무마다 나이를 2 로 나눈 값이 나무가 있던 칸에 양분으로 추가됩니다. 소수점 아래는 버립니다.
- 가을에는 나무가 번식합니다. 번식하는 나무는 나이가 5 의 배수이어야 하며, 인접한 8 개의 칸에 나이가 1 인 나무가 생깁니다.
  - 어떤 칸 (r, c)와 인접한 칸은 (r-1, c-1), (r-1, c), (r-1, c+1), (r, c-1), (r, c+1), (r+1, c-1), (r+1, c), (r+1, c+1) 입니다.
  - 땅을 벗어나는 칸에는 나무가 생기지 않습니다.
- 겨울에는 S2D2 가 땅을 돌아다니면서 땅에 양분을 추가합니다. 각 칸에 추가되는 양분의 양은 A[r][c] 이고, 입력으로 주어집니다.
- K 년이 지난 후 땅에 살아있는 나무의 개수를 구하는 문제입니다.

## 문제 풀이 step 2

- 위의 정보를 기반해서 N x N 크기의 격자에 나무의 정보를 입력하고, 나무 재테크 로직을 적용하는 문제입니다.
- 우선, N x N 크기의 격자를 만들고, 나무 정보를 넣어줍니다.
  - 격자의 각 칸에는 [양분의 양, 1 번 우선순위 큐, 2 번 우선순위 큐] 형태로 들어갑니다.
  - 1 번 우선순위 큐에 나무 정보가 들어갑니다.
  - 2 번 우선순위 큐에는 양분을 먹고 나이가 늘어난 나무 또는 번식을 통해 생성된 나이가 1 인 나무들이 들어갑니다.
- k 번 만큼 봄, 여름, 가을, 겨울 로직을 적용합니다.
  - 봄
    - 1 번 우선순위 큐에서 나이가 어린 나무부터 순서대로 가져옵니다.
    - 가져온 나무의 나이가 양분의 양보다 같거나 작다면, 나무의 나이에 1 을 더한 값을 2 번 우선순위 큐에 넣습니다.
    - 반대로 가져온 나무의 나이가 양분의 양보다 크다면, 이 과정을 멈춥니다.
  - 여름
    - 봄 로직을 적용한 후, 각 칸의 1 번 우선순위 큐에는 양분의 양보다 큰 나무들만 남아있습니다.
    - 이 나무들은 죽어서, 양분이 됩니다.
    - 따라서, 1 번 우선순위 큐에 있는 모든 나무에 대해서, `"각 나무의 나이 / 2"` 에서 소수점 이하를 버린 값을 양분에 추가합니다.
    - 2 번 우선순위 큐에 있는 내용을 1 번 우선순위 큐로 옮깁니다. (이는 참조만 변경해주면 됩니다.)
  - 가을
    - 가을에는 나이가 5 의 배수인 나무가 번식을 해서 8 방향의 칸에 나이가 1 인 나무를 생성시킵니다.
    - 1 번 우선순위 큐에서 나무를 모두 가져옵니다.
    - 가져온 나무의 나이가 5 의 배수라면, 8 방향의 각 칸의 2 번 우선순위 큐에 나이가 1 인 나무를 모두 추가합니다.
    - 가져온 나무는 모두 2 번 우선순위 큐로 옮깁니다.
  - 겨울
    - 문제에서 주어진 각 칸에 추가되는 양분의 양 정보를 토대로 각 칸의 양분의 양을 증가시킵니다.
- 위 과정을 모두 수행한 후에 격자의 각 칸에 살아남은 나무의 개수를 모두 세서 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 저는 우선순위 큐를 이용해서 문제를 풀어봤는데요, 오히려 배열을 사용하는 것이 더 빠른 것 같습니다.
- 배열을 사용하게 되면, 각 원소에 index 를 통해서 바로 접근이 가능하기 때문에 몇몇의 로직을 더욱 효과적으로 작성할 수 있는 것 같습니다.
- 반면 우선순위 큐의 경우, 원소를 확인하기 위해서는 큐에서 원소를 뽑아내는 수 밖에 없기 때문에 오히려 몇몇 로직에 제한이 생기는 것 같습니다.
- 이렇기 때문에, 각 상황에 가장 적합한 자료구조를 쓰는 것이 제일 좋은 것 같습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 우선순위 큐 구현
class MinHeap {
	constructor() {
		this.bucket = [null];
	}

	enqueue(item) {
		let index = this.bucket.length;

		while (index !== 1 && item < this.bucket[parseInt(index / 2)]) {
			this.bucket[index] = this.bucket[parseInt(index / 2)];
			index = parseInt(index / 2);
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

	size() {
		return this.bucket.length - 1;
	}
}

// 봄
const spring = (n, grid) => {
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			let [food, ageMinHeap, nextMinHeap] = grid[i][j];
			if (food === 0 || ageMinHeap.isEmpty()) continue;

			while (!ageMinHeap.isEmpty()) {
				const age = ageMinHeap.dequeue();

				// 나이가 양분의 양보다 크다면, 종료
				if (food < age) {
					ageMinHeap.enqueue(age);
					break;
				}

				// 나이가 양분의 양보다 작다면, 2 번 우선순위 큐에 "나이 + 1" 값 추가
				food -= age;
				nextMinHeap.enqueue(age + 1);
			}

			// 양분의 양 갱신
			grid[i][j][0] = food;
		}
	}
};

// 여름
const summer = (n, grid) => {
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			let [food, ageMinHeap, nextHeap] = grid[i][j];

			// 1 번 우선순위 큐에서
			while (!ageMinHeap.isEmpty()) {
				const age = ageMinHeap.dequeue();

				// 나이 / 2 후 소수점 이하의 값을 버린 값을 양분에 추가
				food += parseInt(age / 2);
			}

			// 2 번 우선순위 큐에 있는 원소 1 번 우선순위 큐로 전달 (참조만 변경)
			grid[i][j][0] = food;
			grid[i][j][1] = nextHeap;
			grid[i][j][2] = new MinHeap();
		}
	}
};

// 총 8 가지 방향
const dir = [
	[-1, -1],
	[-1, 0],
	[-1, 1],
	[0, -1],
	[0, 1],
	[1, -1],
	[1, 0],
	[1, 1],
];

// 불가능한 칸인지 검사하는 함수
const isPossibleRoute = (n, x, y) => 0 <= x && x < n && 0 <= y && y < n;

// 가을
const autumn = (n, grid) => {
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			const [food, ageMinHeap, nextMinHeap] = grid[i][j];

			if (ageMinHeap.isEmpty()) continue;

			// 1 번 우선순위 큐에서
			while (!ageMinHeap.isEmpty()) {
				// 2 번 우선순위 큐로 이동시키며,
				const age = ageMinHeap.dequeue();
				nextMinHeap.enqueue(age);

				// 나무의 나이가 5 의 배수라면 총 8 개의 칸에 나이가 1 인 나무 생성
				if (age % 5 === 0) {
					for (let k = 0; k < 8; k++) {
						const [nx, ny] = [i + dir[k][0], j + dir[k][1]];
						if (!isPossibleRoute(n, nx, ny)) continue;

						// 나이가 1 인 나무는 각 칸의 2 번 우선순위 큐에 넣기
						grid[nx][ny][2].enqueue(1);
					}
				}
			}
		}
	}

	// 2 번 우선순위 큐에 있는 나무 모두 1 번 우선순위 큐로 이동 (참조만 변경)
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			grid[i][j][1] = grid[i][j][2];
			grid[i][j][2] = new MinHeap();
		}
	}
};

// 겨울
const winter = (n, foodArr, grid) => {
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			// 양분 추가
			grid[i][j][0] += foodArr[i][j];
		}
	}
};

const solution = (input) => {
	let [n, m, k] = input[0].split(" ").map(Number);
	const foodArr = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	// N x N 크기의 땅 생성
	const grid = Array.from({length: n}, () => Array.from({length: n}, () => []));
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			grid[i][j].push(5);
			grid[i][j].push(new MinHeap());
			grid[i][j].push(new MinHeap());
		}
	}

	// 나무 정보 입력
	for (let i = n + 1; i < n + 1 + m; i++) {
		const [r, c, age] = input[i].split(" ").map(Number);
		const ageMinHeap = grid[r - 1][c - 1][1];

		ageMinHeap.enqueue(age);
	}

	// k 번만큼 봄, 여름, 가을, 겨울 로직 적용
	while (k-- > 0) {
		spring(n, grid);
		summer(n, grid);
		autumn(n, grid);
		winter(n, foodArr, grid);
	}

	// 살아남은 나무의 개수 세기
	let ans = 0;
	for (let i = 0; i < n; i++) {
		for (let j = 0; j < n; j++) {
			const ageMinHeap = grid[i][j][1];

			ans += ageMinHeap.size();
		}
	}

	return ans;
};

console.log(solution(input));
```
