---
layout: post
title: "BOJ[3085] - 사탕 게임 by JavaScript"
date: 2021-02-24 23:00:00 +0900
categories: BOJ(BruteForce)
---

# 사탕 게임

## 문제

- [백준 3085번 - 사탕 게임](https://www.acmicpc.net/problem/3085)

## 언어

- JavaScript

## 문제 풀이 step 1

- 완전 탐색 문제로 모든 경우의 수를 고려해서 풀면 됩니다. (범위는 충분히 가능한 숫자입니다.)
- 왼쪽 상단에서부터 시작해서 인접한 두 칸의 사탕을 교환합니다. 그리고 모두 같은 색으로 이루어져 있는 가장 긴 연속 부분(행 또는 열)을 골라서 사탕의 개수를 세주면 됩니다.
- 왼쪽 상단에서부터 비교하면서 내려오니까 본인 기준 우측 한 번, 본인 기준 하단 한 번 비교하면서 내려오면 상하좌우 모두 비교하게 됩니다.
- 속도를 향상시킬 수 있는 방법이 있는데 이는 나중에 올리겠습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const swap = (map, i, j, adjacentI, adjacentJ) => {
	let temp = map[i][j];
	map[i][j] = map[adjacentI][adjacentJ];
	map[adjacentI][adjacentJ] = temp;
};

const getMax = (map, N) => {
	let max = 0;

	for (let i = 0; i < N; i++) {
		let row = 1;
		let col = 1;
		for (let j = 0; j < N - 1; j++) {
			if (map[i][j] === map[i][j + 1]) {
				row++;
			} else {
				max = Math.max(max, row);
				row = 1;
			}

			if (map[j][i] === map[j + 1][i]) {
				col++;
			} else {
				max = Math.max(max, col);
				col = 1;
			}

			if (j === N - 2) {
				max = Math.max(max, row, col);
			}
		}
	}

	return max;
};

const solution = (input) => {
	const N = parseInt(input[0]);
	const map = input.slice(1, N + 1).map((v) => v.split(""));

	let max = 0;
	let adjacentI = null;
	let adjacentJ = null;
	const adjacentPlus = [
		[1, 0],
		[0, 1],
	];
	for (let i = 0; i < N; i++) {
		for (let j = 0; j < N; j++) {
			for (let k = 0; k < 2; k++) {
				adjacentI = i + adjacentPlus[k][0];
				adjacentJ = j + adjacentPlus[k][1];

				if (adjacentI < N && adjacentJ < N) {
					swap(map, i, j, adjacentI, adjacentJ);
					max = Math.max(max, getMax(map, N));
					swap(map, i, j, adjacentI, adjacentJ);
				}
			}
		}
	}

	return max;
};

console.log(solution(input));
```
