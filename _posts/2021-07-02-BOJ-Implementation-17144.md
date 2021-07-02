---
layout: post
title: "BOJ[17144] - 미세먼지 안녕! by JavaScript"
date: 2021-07-02 14:00:00 +0900
categories: BOJ(Implementation)
---

# 미세먼지 안녕!

## 문제

- [백준 17144번 - 미세먼지 안녕!](https://www.acmicpc.net/problem/17144)

## 언어

- JavaScript

## 순서도

1. 위쪽 공기청정기의 바람길 찾기
2. 아래쪽 공기청정기의 바람길 찾기
3. 집의 미세먼지 확산
4. 공기 청정기 작동
5. T 번 만큼 3, 4 반복
6. 집의 미세먼지 총합 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 집에 미세먼지와 공기청정기가 있는데, 시간이 지남에 따라 미세먼지는 확산되고, 공기청정기는 미세먼지를 정화합니다.
- 미세먼지에 관한 정보는 다음과 같습니다.
  - 미세먼지의 확산은 미세먼지가 있는 모든 칸에서 동시에 일어납니다.
  - 미세먼지는 인접한 네 방향으로 확산되는데, 인접한 방향에 공기청정기가 있거나, 칸이 없으면 그 방향으로는 확산되지 않습니다.
  - 미세먼지가 확산되는 양은 **`(그 칸의 미세먼지의 양) / 5`** 에서 소수점을 버린 값입니다.
  - 그 칸의 확산되고 남은 미세먼지의 양은 **`(그 칸의 미세먼지의 양) - (미세먼지의 확산양) x (확산된 방향의 개수)`** 입니다.
- 공기청정기에 관한 정보는 다음과 같습니다.
  - 공기청정기에서는 미세먼지가 없는 바람이 나오고, 바람을 빨아들여서 미세먼지를 모두 정화합니다.
  - 공기청정기는 위쪽 공기청정기와 아래쪽 공기청정기가 있습니다.
  - 위쪽 공기청정기의 바람은 반시계방향으로 순환하고, 아래쪽 공기청정기의 바람은 시계방향으로 순환합니다.
  - 공기청정기의 바람이 불면 미세먼지가 바람의 방향대로 모두 한 칸씩 이동합니다.
- 위의 정보들을 이용해서 시간에 따라 집의 미세먼지 상태를 변화시키고, 최종적으로 집의 미세먼지의 총합을 출력하는 문제입니다.

## 문제 풀이 step 2

- 우선 집에서 공기청정기의 위치를 찾고, 위쪽 공기청정기의 바람길과 아래쪽 공기청정기의 바람길을 구합니다.
  - 공기청정기는 항상 1 번 열에 설치되어 있습니다.
  - 나중에 공기청정기의 작동을 구현하기 위해서 각 공기청정기의 바람길을 구합니다.
  - 위쪽 공기청정기는 반시계방향으로, 아래쪽 공기청정기는 시계방향으로 순환합니다.
- 집의 미세먼지를 확산시킵니다.
  - 미세먼지의 확산은 미세먼지가 있는 모든 칸에서 동시에 일어납니다.
  - 따라서 동시에 일어나는 것처럼 구현하기 위해서 **미세먼지의 확산**과 **확산된 미세먼지를 주변 방에 더하는 과정**을 분리합니다.
  - 즉, 확산된 미세먼지를 저장하는 별도의 배열 `spreaded [][]` 를 생성합니다.
  - 미세먼지를 확산할 때마다 확산된 미세먼지를 주변 방에 더하지 않고, 우선 `spreaded [][]` 에 저장합니다.
  - 그리고 각 방의 미세먼지를 모두 확산시킨 후에, 각 방에 `spreaded[][]` 의 값들을 더해줍니다.
- 위쪽 공기청정기와 아래쪽 공기청정기를 작동시킵니다.
  - 아까 구한 바람길에 따라서 위쪽 공기청정기의 바람을 순환시키고, 아래쪽 공기청정기의 바람을 순환시킵니다.
- 문제에서 주어진 T 시간 만큼 미세먼지를 확산시키고, 공기청정기를 작동시킵니다.
- T 시간이 지난 후에 집에 최종적으로 남아있는 미세먼지를 모두 더해서 출력하면 정답이 됩니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 미세먼지가 확산될 칸이 집을 벗어나는지 검사하는 함수
const isPossibleRoute = (r, c, x, y) => 0 <= x && x < r && 0 <= y && y < c;

// 미세먼지를 확산시키는 함수
const spreadDust = (r, c, home) => {
	// 각 방에서 미세먼지가 동시에 확산하는 것처럼 구현하기 위해서
	// 확산된 미세먼지를 저장하는 별도의 배열 생성
	const spreaded = Array.from({length: r}, () => Array(c).fill(0));

	const cx = [0, 0, 1, -1];
	const cy = [1, -1, 0, 0];

	for (let i = 0; i < r; i++) {
		for (let j = 0; j < c; j++) {
			// 미세먼지 양이 5 보다 작으면 확산이 일어나지 않습니다.
			if (home[i][j] < 5) continue;

			let cnt = 0;
			// 확산된 미세먼지 양 구하기
			const dust = parseInt(home[i][j] / 5);
			// 인접한 네 방향으로 미세먼지 확산
			for (let k = 0; k < 4; k++) {
				const [nx, ny] = [i + cx[k], j + cy[k]];

				// 집을 벗어나거나 인접한 방에 공기청정기가 있다면 확산 불가능
				if (!isPossibleRoute(r, c, nx, ny)) continue;
				if (home[nx][ny] === -1) continue;

				// 별도의 배열에 우선 확산된 미세먼지 저장
				spreaded[nx][ny] += dust;
				cnt++;
			}

			// 각 방에는 미세먼지 확산 후 결과만 기록
			home[i][j] -= dust * cnt;
		}
	}

	// 확산된 미세먼지를 다시 각 방에 더해주기
	for (let i = 0; i < r; i++) {
		for (let j = 0; j < c; j++) {
			home[i][j] += spreaded[i][j];
		}
	}
};

// 공기청정기의 바람길을 입력받아서 공기청정기를 작동시키는 함수
const clear = (home, airCleanerRoute) => {
	// 공기청정기의 바람길에 따라서 공기 순환시키기
	for (let i = 1; i < airCleanerRoute.length - 1; i++) {
		const [x, y] = airCleanerRoute[i];
		const [nx, ny] = airCleanerRoute[i + 1];
		home[x][y] = home[nx][ny];
	}

	// 공기청정기에서 나오는 바람은 미세먼지가 없는 바람
	const [x, y] = airCleanerRoute[airCleanerRoute.length - 1];
	home[x][y] = 0;
};

// 각 방의 미세먼지를 더하는 함수
const sumDusts = (r, c, home) => {
	let sum = 2;
	for (let i = 0; i < r; i++) {
		for (let j = 0; j < c; j++) {
			sum += home[i][j];
		}
	}

	return sum;
};

const solution = (input) => {
	let [r, c, t] = input[0].split(" ").map(Number);
	const home = input.slice(1, r + 1).map((v) => v.split(" ").map(Number));

	const topAirCleanerRoute = [];
	const botAirCleanerRoute = [];
	for (let i = 0; i < r; i++) {
		if (home[i][0] !== -1) continue;

		// 위의 공기청정기의 바람길 구하기
		for (let j = i; j >= 0; j--) topAirCleanerRoute.push([j, 0]);
		for (let j = 1; j < c; j++) topAirCleanerRoute.push([0, j]);
		for (let j = 1; j <= i; j++) topAirCleanerRoute.push([j, c - 1]);
		for (let j = c - 2; j > 0; j--) topAirCleanerRoute.push([i, j]);

		// 아래의 공기청정기의 바람길 구하기
		for (let j = i + 1; j < r; j++) botAirCleanerRoute.push([j, 0]);
		for (let j = 1; j < c; j++) botAirCleanerRoute.push([r - 1, j]);
		for (let j = r - 2; j >= i + 1; j--) botAirCleanerRoute.push([j, c - 1]);
		for (let j = c - 2; j > 0; j--) botAirCleanerRoute.push([i + 1, j]);

		break;
	}

	// 주어진 시간 T 만큼 반복
	while (t-- > 0) {
		// 미세먼지 확산
		spreadDust(r, c, home);

		// 위의 공기청정기와 아래의 공기청정기 작동
		clear(home, topAirCleanerRoute);
		clear(home, botAirCleanerRoute);
	}

	// 최종적으로 집에 남아있는 미세먼지의 양 출력
	return sumDusts(r, c, home);
};

console.log(solution(input));
```
