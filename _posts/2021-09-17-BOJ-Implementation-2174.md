---
layout: post
title: "BOJ[2174] - 로봇 시뮬레이션 by JavaScript"
date: 2021-09-17 12:00:00 +0900
categories: BOJ(Implementation)
---

# 로봇 시뮬레이션

## 문제

- [백준 2174번 - 로봇 시뮬레이션](https://www.acmicpc.net/problem/2174)

## 언어

- JavaScript

## 순서도

1. A x B 크기의 보드에 로봇의 위치 기록, 각 로봇의 정보 수집
2. 각 명령어에 맞게 로봇에 명령 주입
   1. 도중에 벽에 박거나 다른 로봇에 박으면 해당 내용 출력
   2. 아무 문제없이 명령이 수행되면 "OK" 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 로봇들에게 명령을 시뮬레이션하며 안전성을 검증하는 프로그램을 작성하는 문제입니다.
  - 가로 A (1 ~ 100), 세로 B (1 ~ 100) 크기의 땅이 있습니다. 그리고 로봇이 N (1 ~ 100) 개 있습니다.
  - 로봇들의 초기 위치는 x, y 좌표로 나타내고, x 좌표는 왼쪽부터, y 좌표는 아래쪽부터 순서가 매겨져있습니다.
  - 각 로봇은 맨 처음에 NWES 중 하나의 방향을 향해 서 있고, 초기에 서 있는 로봇들의 위치는 서로 다릅니다.
  - 이러한 로봇들에 M (1 ~ 100) 개의 명령을 내리려고 하고, 각각의 명령은 순차적으로 실행됩니다.
  - 즉, 하나의 명령을 한 로봇에 내렸으면, 그 명령이 완수될 때까지 그 로봇과 다른 모든 로봇에게 다른 명령을 내릴 수 없습니다.
  - 각각의 로봇에 대해 수행하는 명령은 다음 3 가지가 있습니다.
    1. L : 로봇이 향하고 있는 방향을 기준으로 왼쪽으로 90 도
    2. R : 로봇이 향하고 있는 방향을 기준으로 오른쪽으로 90 도
    3. F : 로봇이 향하고 있는 방향을 기준으로 앞으로 한 칸 이동
  - 잘못된 명령은 두 가지 있습니다.
    1. X 번 로봇이 벽에 충돌하는 경우, 즉 주어진 땅의 밖으로 벗어나는 경우
    2. X 번 로봇이 움직이다가 Y 번 로봇에 충돌하는 경우

## 문제 풀이 step 2

- 우선, A x B 크기의 2 차원 배열과 로봇의 정보를 기록하는 배열을 생성합니다.
  - 2 차원 배열에는 각 로봇의 위치를 기록합니다.
  - 1 차원 배열에는 각 로봇의 좌표와 방향을 기록합니다.
- 그리고 로봇의 방향을 쉽게 전환하고, 로봇의 방향을 이용해서 전진할 수 있도록 편리한 배열을 생성합니다.
  - [[1, 0], [0, 1], [-1, 0], [0, -1]]
  - 순서대로 북, 동, 남, 서 입니다.
- 그리고 각 명령에 맞게 로봇에 시뮬레이션 하면 됩니다.
  - L 명령은 로봇의 방향을 왼쪽으로 90 도 회전하는 명령으로,
    - 만약 로봇의 현재 방향이 [1, 0] 이라면 [0, -1] 이 되고, [0, -1] 이라면 [-1, 0] 이 되도록 구현합니다.
  - R 명령은 로봇의 방향을 오른쪽으로 90 도 회전하는 명령으로,
    - 만약 로봇의 현재 방향이 [1, 0] 이라면, [0, 1] 이 되고, [0, -1] 이라면 [1, 0] 이 되도록 구현합니다.
  - F 명령은 로봇을 한 칸 전진시키는 명령으로,
    - 로봇의 좌표를 현재 방향을 기준으로 한 칸 이동시키고,
    - 만약 벽에 박으면 `Robot ${num} crashes into the wall` 을 출력하고
    - 만약 다른 로봇에 박으면 `Robot ${num} crashes into robot ${board[x][y]}` 을 출력합니다.
- 모든 명령을 시뮬레이션 한 후에도 아무런 문제가 발생하지 않았다면 `OK` 를 출력합니다.

## 후기

- 본 문제를 풀기 전에 정답 비율이 22% 라서 많은 시행착오를 겪겠구나 하고 마음을 먹고 문제를 풀었는데, 문제의 난이도 때문은 아닌 것 같습니다.
- 주어지는 입력이 x 좌표와 y 좌표가 거꾸로 되어 있어서, 그 부분에서 실수가 발생해서 정답 비율이 내려간 것 같습니다.
- 그리고 숫자와 문자를 동시에 입력으로 주어질 때는 숫자는 number 형으로 바꾸는 작업을 꼭 기억해야겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 벽을 박았는지 검사하는 함수
const isPossibleRoute = (a, b, x, y) => 0 <= x && x < b && 0 <= y && y < a;

const solution = (input) => {
	// 가로 길이: a, 세로 길이: b, 로봇의 개수: n, 명령의 개수: m
	const [a, b] = input[0].split(" ").map(Number);
	const [n, m] = input[1].split(" ").map(Number);

	// 4 개의 방향 정의
	const directions = [
		[1, 0],
		[0, 1],
		[-1, 0],
		[0, -1],
	];
	const initDir = {N: 0, E: 1, S: 2, W: 3};

	const robots = []; // [[x, y, dir], [x, y, dir], ...]
	const board = Array.from({length: b}, () => Array(a).fill(-1));
	for (let i = 2; i < 2 + n; i++) {
		// y 가 세로, x 가 가로
		let [y, x, dir] = input[i].split(" ");
		[y, x] = [Number(y), Number(x)];
		const num = i - 1;

		robots[num] = [+x - 1, +y - 1, initDir[dir]];
		board[+x - 1][+y - 1] = num;
	}

	for (let i = 2 + n; i < 2 + n + m; i++) {
		let [num, command, repeat] = input[i].split(" ");
		[num, repeat] = [Number(num), Number(repeat)];

		let [x, y, dir] = robots[num];
		board[x][y] = -1;

		for (let j = 0; j < repeat; j++) {
			if (command === "L") {
				// 왼쪽으로 방향 전환
				dir -= 1;
				if (dir < 0) dir = 3;
			} else if (command === "R") {
				// 오른쪽으로 방향 전환
				dir += 1;
				if (dir > 3) dir = 0;
			} else {
				// 로봇 한 칸 이동
				[x, y] = [x + directions[dir][0], y + directions[dir][1]];

				// 벽에 박은 경우
				if (!isPossibleRoute(a, b, x, y))
					return `Robot ${num} crashes into the wall`;
				// 다른 로봇에 박은 경우
				if (board[x][y] !== -1)
					return `Robot ${num} crashes into robot ${board[x][y]}`;
			}
		}

		// 로봇의 위치 갱신 및 좌표 와 방향 갱신
		board[x][y] = num;
		robots[num] = [x, y, dir];
	}

	// 위의 시뮬레이션 중에 아무 문제가 발생하지 않았다면 "OK" 출력
	return "OK";
};

console.log(solution(input));
```
