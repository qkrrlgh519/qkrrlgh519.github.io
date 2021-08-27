---
layout: post
title: "BOJ[17135] - 캐슬 디펜스 by JavaScript"
date: 2021-08-27 12:00:00 +0900
categories: BOJ(Implementation)
---

# 캐슬 디펜스

## 문제

- [백준 17135번 - 캐슬 디펜스](https://www.acmicpc.net/problem/17135)

## 언어

- JavaScript

## 순서도

1. m 개 중에서 3 개를 고르는 모든 경우 구하기
2. 각 경우마다, 궁수가 적을 제거하는 횟수 세기
   1. 모든 적에 대해서 각 궁수로부터의 거리 구하기
   2. 현재 궁수로부터 공격 대상이 되는 적 찾기
   3. 공격 대상이 되는 적들을 제거하고, 궁수가 적을 제거한 횟수 갱신
   4. 적들 모두 한 칸씩 아래로 내려오기
   5. 격자에서 모든 적이 사라질 때까지 2, 3, 4, 5 번 반복하기
3. 각 경우의 궁수가 적을 제거한 횟수 중에서 최댓값 구하기

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 캐슬 디펜스는 성을 향해 몰려오는 적을 잡는 턴 방식의 게임입니다.
  - N x M 크기의 격자판으로 구성되고, 격자판은 1 x 1 크기의 칸으로 나누어져 있습니다.
  - 각 칸에 포함된 적의 수는 최대 하나입니다.
  - 격자판 N 번 행의 바로 아래 N + 1 번 행의 모든 칸에는 성이 있습니다.
  - 성을 지키기 위해 궁수 3명을 배치해야 하고, 궁수는 성이 있는 칸에 최대 1 명의 궁수만 배치할 수 있습니다.
  - 각각의 턴마다 궁수는 적 하나를 공격하고, 모든 궁수는 동시에 공격합니다.
  - 궁수가 공격하는 적은 거리가 D 이하인 거 중에서 가장 가까운 적을 그리고 그러한 적이 여럿일 경우 가장 왼쪽에 있는 적을 공격합니다.
  - 같은 적이 여러 궁수에게 공격당할 수 있고, 공격받은 적은 게임에서 제외됩니다.
  - 궁수의 공격이 끝나면, 적은 아래로 한 칸 이동하며, 성이 있는 칸으로 이동할 경우 게임에서 제외됩니다.
  - 모든 적이 격자판에서 제외되면 게임은 종료됩니다.

## 문제 풀이 step 2

- 위의 규칙을 이용해서 적절히 궁수 3명을 성벽에 배치하고, 궁수의 공격으로 제거할 수 있는 적의 최대값을 구하는 문제입니다.
- 우선, 격자 판의 가로 길이인 m 값을 이용해서, m 중에서 3 개를 고르는 모든 경우를 만듭니다.
  - 이는 m 칸에 궁수 3 명을 배치하는 모든 경우의 수를 구하는 것을 의미합니다.
- 그리고 각 경우마다,
  - 격자 판의 모든 적에 대해서 각 궁수로부터의 거리를 구합니다.
  - 궁수마다 궁수의 공격 거리 내부에 위치한 적 중에서 가장 가까우며 가장 왼쪽인 적을 찾습니다.
  - 적을 제거하고, 궁수가 죽인 적의 횟수를 갱신합니다.
  - 적을 아래로 한 칸 이동시킵니다.
  - 위 과정을 격자판에서 모든 적이 제거될 때까지 반복합니다.
- 각 경우의 궁수가 죽인 적의 횟수 중에서 최댓값을 찾아서 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- m 개 중에서 3 개를 골라야 하는데, 실수로 n 개 중에서 3 개를 고르는 바람에 삽질을 했습니다.
  - 범위를 명확하게 정할 필요가 있겠습니다.
- 배열을 복사할 때 slice() 함수를 쓰면 쉽게 깊은 복사를 할 수 있습니다.
  - 2 차원 배열의 복사는 `originalArray.map((v) => v.slice());` 코드로 간편하게 해결할 수 있습니다.
- 2 차원 배열을 생성할 때,
  - `Array.from({length: n}, Array(m).fill([]));` 코드는 내부 배열이 참조가 같은 배열이 생성되고,
  - `Array.from({length: n}, Array.from({length: m}, () => []));` 코드는 내부 배열이 참조가 다른 배열이 생성됩니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 모든 적에 대해서 각 궁수로부터의 거리를 구하는 함수
const getDistance = (n, m, grid, archer) => {
	const distance = Array.from({length: n}, () =>
		Array.from({length: m}, () => [])
	);

	for (let i = 0; i < n; i++) {
		for (let j = 0; j < m; j++) {
			if (grid[i][j] === 0) continue;

			for (let k = 0; k < 3; k++) {
				distance[i][j][k] = Math.abs(n - i) + Math.abs(j - archer[k]);
			}
		}
	}

	return distance;
};

// 궁수가 죽인 적의 횟수 구하는 함수
const countDeadEnemy = (n, m, d, grid, archer) => {
	let count = 0;

	// 각 궁수로부터 거리 구하기
	const distance = getDistance(n, m, grid, archer);

	let round = 0;
	// 적의 한칸 아래로의 이동 구현 : 궁수의 위치를 한 칸씩 위로 올림
	for (let i = n - 1; i >= 0; i--) {
		const target = [null, null, null];

		// 궁수의 공격 거리 내부에 있는 적 찾기
		for (let k = 0; k < m; k++) {
			for (let j = i; j > i - d; j--) {
				if (j < 0) break;

				if (grid[j][k] === 0) continue;

				for (let l = 0; l < 3; l++) {
					// 적의 한칸 아래로의 이동 구현: round 변수를 빼주기
					if (distance[j][k][l] - round > d) continue;

					if (target[l] === null) target[l] = [j, k];

					const [beforeX, beforeY] = target[l];
					if (distance[beforeX][beforeY][l] > distance[j][k][l])
						target[l] = [j, k];
				}
			}
		}

		// 궁수의 공격 거리 내부에 있는 적 제거하기
		for (let l = 0; l < 3; l++) {
			if (target[l] === null) continue;

			const [x, y] = target[l];
			if (grid[x][y] === 0) continue;

			grid[x][y] = 0;
			count += 1;
		}

		// 모든 적 아래로 한 칸 이동하기
		round += 1;
	}

	return count;
};

let max = 0;

const rec = (n, m, d, grid, archer, index, depth) => {
	if (depth === 3) {
		const copiedGrid = grid.map((v) => v.slice());
		// 궁수가 죽인 적의 횟수의 최댓값 찾기
		max = Math.max(max, countDeadEnemy(n, m, d, copiedGrid, archer));
		return;
	}

	if (index >= m) return;

	archer.push(index);
	rec(n, m, d, grid, archer, index + 1, depth + 1);

	archer.pop();
	rec(n, m, d, grid, archer, index + 1, depth);
};

const solution = (input) => {
	const [n, m, d] = input[0].split(" ").map(Number);
	const grid = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));

	const archer = [];
	rec(n, m, d, grid, archer, 0, 0);

	return max;
};

console.log(solution(input));
```
