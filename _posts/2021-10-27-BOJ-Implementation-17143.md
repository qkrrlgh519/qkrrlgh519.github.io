---
layout: post
title: "BOJ[17143] - 낚시왕 by JavaScript"
date: 2021-10-27 12:00:00 +0900
categories: BOJ(Implementation)
---

# 낚시왕

## 문제

- [백준 17143번 - 낚시왕](https://www.acmicpc.net/problem/17143)

## 언어

- JavaScript

## 순서도

1. N x N 크기의 격자 생성 후, 상어 정보 입력
2. 낚시왕이 격자의 오른쪽 행 끝까지 도달할 때까지 낚시 및 상어의 이동 구현

## 문제 풀이 step 1

- 낚시왕이 상어 낚시를 하는 곳은 크기가 R x C 인 격자판입니다.
- 격자판의 각 칸은 (r, c) 로 나타낼 수 있고, r 은 행, c 는 열이고, (r, c) 는 격자의 가장 오른쪽 아래에 있는 칸입니다.
- 각 칸에는 상어가 최대 한 마리 들어있을 수 있고, 상어는 크기와 속도를 가지고 있습니다.
- 낚시왕은 처음에 1 번 열의 한 칸 왼쪽에 있습니다. 다음은 1 초 동알 일어나는 일이며, 아래 적힌 순서대로 일어납니다.
  - 낚시왕이 오른쪽으로 한 칸 이동한다.
  - 낚시왕이 있는 열에 있는 상어 중에서 땅과 제일 가까운 상어를 잡습니다. 상어를 잡으면 격자판에서 잡은 상어가 사라진다.
  - 상어가 이동한다.
- 낚시왕은 가장 오른쪽 열의 오른쪽 칸에 이동하면 이동을 멈춥니다.
- 상어는 입력으로 주어진 속도로 이동하고, 속도의 단위는 `"칸/초"` 입니다. 상어가 이동하려고 하는 칸이 격자판의 경계를 넘는 경우에는 방향을 반대로 바꿔서 속력을 유지한채로 이동합니다.
- 상어가 이동을 마친 후에 한 칸에 상어가 두 마리 이상 있을 수 없습니다. 이 때는 크기가 가장 큰 상어가 나머지 상어를 모두 잡아먹습니다.
- 낚시왕이 상어 낚시를 하는 격자판의 상태가 주어졌을 때, 낚시왕이 잡은 상어 크기의 합을 구하는 문제입니다.

## 문제 풀이 step 2

- 위의 정보를 기반해서 R x C 크기의 격자에 상어의 정보를 입력하고, 낚시왕의 낚시와 상어들의 이동을 구현하는 문제입니다.
- 우선, R x C 크기의 격자를 생성하고, 상어의 정보를 입력합니다.
- 그리고 낚시왕이 격자의 왼쪽 끝에서 오른쪽 끝에 도달할 때까지 아래의 과정을 반복합니다.
  - 낚시
    - 현재 낚시왕이 있는 열에서 가장 가까운 상어를 잡습니다. 이와 동시에 낚시왕이 잡은 상어의 크기의 합을 갱신합니다.
    - 낚시왕은 -1 번 행에서 오른쪽으로만 이동하므로, 0 ~ R - 1 까지의 행만 검사하면 됩니다.
  - 상어 이동
    - 격자의 각 칸에 있는 상어를 속력, 이동 방향 정보를 이용해서 이동시킵니다.
    - 속력의 크기에 따라서 불필요한 이동이 생길 수 있으니, 나머지 연산을 통해서 의미있는 이동만 하도록 줄여줍니다.
    - 상어가 벽에 도달하면, 반대 방향으로 이동하는 로직을 추가해서 적절히 상어의 이동 로직을 구현합니다.
    - 상어가 도착한 칸에 이동을 마친 다른 상어가 있다면, 크기 비교를 통해서 크기가 더 큰 상어가 위치하도록 구현합니다.
- 낚시왕이 결국 격자의 오른쪽 끝에 도달하게 되면, 그 때의 잡은 상어 크기의 총합을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 제가 푼 방식은 로직을 명확하게 나누기 위해서, 낚시 로직과 상어의 이동 로직을 분리했습니다.
- 하지만 둘의 로직을 합치게되면, 낚시왕이 가장 가까운 상어의 위치를 찾는 과정을 생략할 수 있어서 더 빠른 코드를 작성할 수 있습니다.
- 이 두 가지의 방법 중에서 합의점을 찾는 것이 정말 중요할 것 같습니다.
- 이 부분이 요즘 가장 고민이 되는 부분입니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 낚시 로직
const fishing = (r, c, grid, fishingKing) => {
	let fishSize = 0;

	// 현재 낚시왕의 위치에서 가장 가까운 상어 낚기
	for (let i = 0; i < r; i++) {
		if (grid[i][fishingKing] !== null) {
			const [s, d, z] = grid[i][fishingKing];
			grid[i][fishingKing] = null;

			fishSize = z;
			break;
		}
	}

	// 낚은 물고기의 크기 반환
	return fishSize;
};

// 1: 위, 2: 아래, 3: 오른쪽, 4: 왼쪽
const dir = [null, [-1, 0], [1, 0], [0, 1], [0, -1]];

// 상어의 이동 로직
const moveAndEat = (r, c, grid) => {
	// 상어의 이동을 구현하기 위한 다음 격자 배열
	const nextGrid = Array.from({length: r}, () =>
		Array.from({length: c}, () => null)
	);

	for (let i = 0; i < r; i++) {
		for (let j = 0; j < c; j++) {
			if (grid[i][j] === null) continue;

			// 속력, 이동 방향, 크기
			let [x, y, s, d, z] = [i, j, ...grid[i][j]];

			// move
			let dist = s;
			if (d <= 2) {
				// 이동 방향이 상, 하 라면
				x += dir[d][0] * dist;
				if (x < 0) d = 2;
				if (x >= r) d = 1;

				// 벽에 닿으면 이동 방향 바꿔서 이동
				while (x < 0 || r <= x) {
					if (x < 0) x = Math.abs(x);
					else if (x >= r) x = r - 1 - (x - (r - 1));

					if (x < 0) d = 2;
					else if (r <= x) d = 1;
				}
			} else {
				// 이동 방향이 좌, 우 라면
				y += dir[d][1] * dist;
				if (y < 0) d = 3;
				if (y >= c) d = 4;

				while (y < 0 || c <= y) {
					if (y < 0) y = Math.abs(y);
					else if (y >= c) y = c - 1 - (y - (c - 1));

					if (y < 0) d = 3;
					else if (c <= y) d = 4;
				}
			}

			// eat
			if (nextGrid[x][y] === null) {
				nextGrid[x][y] = [s, d, z];
			} else {
				// 이동한 칸에 다른 상어가 있다면, 크기 비교 후 더 큰 상어로 갱신
				if (z > nextGrid[x][y][2]) {
					nextGrid[x][y] = [s, d, z];
				}
			}
		}
	}

	// 상어의 이동을 구현한 다음 격자 배열 반환
	return nextGrid;
};

const solution = (input) => {
	const [r, c, m] = input[0].split(" ").map(Number);
	if (m === 0) return 0;

	let grid = Array.from({length: r}, () => Array.from({length: c}, () => null));
	for (let i = 1; i < 1 + m; i++) {
		let [x, y, s, d, z] = input[i].split(" ").map(Number);

		// 의미없는 이동을 없애기 위해서 나머지 연산 적용
		if (d <= 2 && s !== (r - 1) * 2) s %= (r - 1) * 2;
		if (d > 2 && s !== (c - 1) * 2) s %= (c - 1) * 2;

		grid[x - 1][y - 1] = [s, d, z];
	}

	let fishingKing = -1;
	let sum = 0;
	while (true) {
		fishingKing += 1;

		// 낚시 후 낚은 물고기의 크기 갱신
		sum += fishing(r, c, grid, fishingKing);

		if (fishingKing === c - 1) break;

		// 상어의 이동
		grid = moveAndEat(r, c, grid);
	}

	return sum;
};

console.log(solution(input));
```
