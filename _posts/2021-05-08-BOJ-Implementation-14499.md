---
layout: post
title: "BOJ[14499] - 주사위 굴리기 by JavaScript"
date: 2021-05-08 12:00:00 +0900
categories: BOJ(Implementation)
---

# 주사위 굴리기

## 문제

- [백준 14499번 - 주사위 굴리기](https://www.acmicpc.net/problem/14499)

## 언어

- JavaScript

## 순서도

1. 주사위를 굴리는 함수 구현
2. 주사위 정보와 지도 정보 구현
3. 주어진 명령에 따라서 주사위 굴리기

## 문제 풀이 step 1

- 구현 문제로서, 문제에서 주어진 설명대로 구현하는 문제입니다.
- 우선 주사위를 어떻게 굴려야 하며, 굴렸을 때 주사위 정보는 어떻게 저장을 해야하는지 고민을 해야합니다.
- 저는 문제에서 제공된 주사위의 전개도 사진을 참고해서 주사위 정보를 저장했습니다.
- 2 차원 배열을 이용해서, 주사위의 전개도 모양 만큼만 주사위 정보를 저장했습니다.
  ![백준 14499번 문제 주사위 초기 정보](/public/img/BOJ-Implementation/BOJ-14499-1.JPG)
  - 이 때, 바닥면은 (1, 1) 이고, 상단면은 (3, 1) 입니다.

## 문제 풀이 step 2

- 주사위 정보를 저장한 후에, 주사위를 굴리는 로직을 구현해야 합니다.
- **1 번 명령 : 동쪽 이동**
  ![백준 14499번 문제 주사위 동쪽 이동](/public/img/BOJ-Implementation/BOJ-14499-2.JPG)
  - 첫 번째 사진에서 주사위를 동쪽으로 굴리면 이런 모양이 나옵니다.
  - 왼쪽에는 주사위에 적힌 수의 이동 변화를, 오른쪽에는 좌표값의 이동 변화를 적었습니다.
  - 이를 소스코드로 나타내면,
  ```jsx
  [dice[1][0], dice[1][1], dice[1][2], dice[3][1]] = [
  	dice[1][1],
  	dice[1][2],
  	dice[3][1],
  	dice[1][0],
  ];
  ```
- **2번 명령 : 서쪽 이동**
  ![백준 14499번 문제 주사위 서쪽 이동](/public/img/BOJ-Implementation/BOJ-14499-3.JPG)
  - 첫 번째 사진에서 주사위를 서쪽으로 굴리면 이런 모양이 나옵니다.
  ```jsx
  [dice[1][0], dice[1][1], dice[1][2], dice[3][1]] = [
  	dice[3][1],
  	dice[1][0],
  	dice[1][1],
  	dice[1][2],
  ];
  ```
- **3번 명령 : 북쪽 명령**
  ![백준 14499번 문제 주사위 북쪽 이동](/public/img/BOJ-Implementation/BOJ-14499-4.JPG)
  - 첫 번째 사진에서 주사위를 북쪽으로 굴리면 이런 모양이 나옵니다.
  ```jsx
  [dice[0][1], dice[1][1], dice[2][1], dice[3][1]] = [
  	dice[3][1],
  	dice[0][1],
  	dice[1][1],
  	dice[2][1],
  ];
  ```
- **4번 명령 : 남쪽 명령**
  ![백준 14499번 문제 주사위 남쪽 이동](/public/img/BOJ-Implementation/BOJ-14499-5.JPG)
  - 첫 번째 사진에서 주사위를 남쪽으로 굴리면 이런 모양이 나옵니다.
  ```jsx
  [dice[0][1], dice[1][1], dice[2][1], dice[3][1]] = [
  	dice[1][1],
  	dice[2][1],
  	dice[3][1],
  	dice[0][1],
  ];
  ```

## 문제 풀이 step 3

- 주사위를 굴리는 로직을 다 구현했으면, 주어진 명령에 맞게 주사위를 굴리면서 주사위의 상단면을 기록해서 출력하면 정답입니다.
- 이 때, 다음과 같은 조건들이 있는데,
  - 주사위와 맞닿은 지도의 수가 0 이면, 주사위의 바닥면의 수가 지도로 복사
  - 주사위와 맞닿은 지도의 수가 0 이 아니면, 지도의 수가 바닥면으로 복사 후 지도의 수는 0 으로 변환
- 위 조건들을 만족하면서 주사위를 굴리면 되겠습니다.

## 후기

- 문제를 풀기 시작할 때는 정말 막막했는데, 문제 안에 힌트가 있었습니다.
- 바로 주사위 전개도 사진입니다. 이 사진 덕분에 풀 수 있었습니다.
- 이런 유형의 문제는 실제로 그려보면서, 배열의 idnex 변화 추이를 살펴보면 문제 해결의 열쇠에 가까워지는 것 같습니다.
- 고민하는 과정 속에서 떠오르는 아이디어들을 실현하기 어려울 것 같다고 생각해서 섣부르게 버렸었는데, 우선은 적어놓는 것이 문제 해결에 더욱 가까워지는 것 같습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const isPossibleRoute = (n, m, x, y) => 0 <= x && x < n && 0 <= y && y < m;

const moveDice = (dice, command) => {
	if (command <= 1) {
		[dice[1][0], dice[1][1], dice[1][2], dice[3][1]] = [
			dice[1][1],
			dice[1][2],
			dice[3][1],
			dice[1][0],
		];
	} else if (command <= 2) {
		[dice[1][0], dice[1][1], dice[1][2], dice[3][1]] = [
			dice[3][1],
			dice[1][0],
			dice[1][1],
			dice[1][2],
		];
	} else if (command <= 3) {
		[dice[0][1], dice[1][1], dice[2][1], dice[3][1]] = [
			dice[3][1],
			dice[0][1],
			dice[1][1],
			dice[2][1],
		];
	} else {
		[dice[0][1], dice[1][1], dice[2][1], dice[3][1]] = [
			dice[1][1],
			dice[2][1],
			dice[3][1],
			dice[0][1],
		];
	}
};

const solution = (input) => {
	let [n, m, x, y, k] = input[0].split(" ").map(Number);
	let arr = input.slice(1, n + 1).map((v) => v.split(" ").map(Number));
	let commands = input[n + 1].split(" ").map(Number);

	// 주사위 초기화
	const dice = [
		[null, 0, null],
		[0, 0, 0],
		[null, 0, null],
		[null, 0, null],
	];
	let diceBot = [1, 1];
	let diceTop = [3, 1];

	// 이동 정의
	const cx = [null, 0, 0, -1, 1];
	const cy = [null, 1, -1, 0, 0];

	// 각 명령에 맞게 이동
	let res = "";
	for (let i = 0; i < k; i++) {
		const command = commands[i];
		const [nx, ny] = [x + cx[command], y + cy[command]];

		// 지도에서 이동을 통한 좌표 변경
		if (!isPossibleRoute(n, m, nx, ny)) continue;
		[x, y] = [nx, ny];

		// 주사위 이동
		moveDice(dice, command);

		if (arr[x][y] === 0) {
			// 주사위와 맞닿은 지도의 수가 0 이면, 주사위의 바닥면의 수가 지도로 복사
			arr[x][y] = dice[diceBot[0]][diceBot[1]];
		} else {
			// 주사위와 맞닿은 지도의 수가 0 이 아니면, 지도의 수가 바닥면으로 복사 후 지도의 수는 0
			dice[diceBot[0]][diceBot[1]] = arr[x][y];
			arr[x][y] = 0;
		}

		// 이때의 top 값을 res 에 추가
		res += dice[diceTop[0]][diceTop[1]] + "\n";
	}

	return res;
};

console.log(solution(input));
```
