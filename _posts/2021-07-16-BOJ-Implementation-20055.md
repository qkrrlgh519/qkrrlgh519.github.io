---
layout: post
title: "BOJ[20055] - 컨베이어 벨트 위의 로봇 by JavaScript"
date: 2021-07-16 13:00:00 +0900
categories: BOJ(Implementation)
---

# 컨베이어 벨트 위의 로봇

## 문제

- [백준 20055번 - 컨베이어 벨트 위의 로봇](https://www.acmicpc.net/problem/20055)

## 언어

- JavaScript

## 순서도

1. 벨트가 각 칸 위에 있는 로봇과 함께 회전
2. 가장 먼저 벨트에 올라간 로봇부터, 벨트가 회전하는 방향으로 한 칸 이동
3. 올리는 위치에 있는 칸의 내구도가 0이 아니면 올리는 위치에 로봇 올리기
4. 내구도가 0 인 칸의 개수가 K 개 이상이라면 과정을 종료. 그렇지 않으면 1 번으로 돌아가기
5. 과정 종료 시의 단계 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 문제에서 주어진 로봇을 옮기는 과정에 따라서 똑같이 동작하도록 구현하는 문제입니다.
- 우선, 입력에 따라서 위의 벨트와 아래의 벨트룰 구현합니다.
- 로봇을 옮기는 과정을 따라하며 각 단계를 계산하며 반복합니다.
- 로봇을 더이상 옮길 수 없는 상태가 되면, 여태까지의 단계의 수를 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const [n, k] = input[0].split(" ").map(Number);
	const arr = input[1].split(" ").map(Number);
	const upBelt = [];
	const downBelt = [];
	const robotBelt = Array(n).fill(false);

	// 입력에 따라서 위의 벨트와 아래의 벨트 구현
	for (let i = 0; i < n; i++) upBelt.push(arr[i]);
	for (let i = n; i < n * 2; i++) downBelt.unshift(arr[i]);

	let cnt = 0;
	while (true) {
		// 단계 갱신
		cnt += 1;

		// 1. 벨트가 각 칸위에 있는 로봇과 함께 한 칸 회전
		upBelt.unshift(downBelt.shift());
		downBelt.push(upBelt.pop());
		robotBelt.pop();
		robotBelt.unshift(false);
		robotBelt[n - 1] = false;

		// 2. 가장 먼저 벨트에 올라간 로봇부터, 벨트가 회전하는 방향으로 한칸 이동 가능하면 이동
		for (let i = n - 1; i > 0; i--) {
			if (upBelt[i] < 1) continue;
			if (robotBelt[i] === true) continue;
			if (robotBelt[i - 1] === false) continue;

			upBelt[i] -= 1;
			robotBelt[i] = true;
			robotBelt[i - 1] = false;
		}
		robotBelt[n - 1] = false;

		// 3. 올리는 위치에 있는 칸의 내구도가 0 이 아니면 올리는 위치에 로봇을 올린다.
		if (robotBelt[0] === false && upBelt[0] > 0) {
			upBelt[0] -= 1;
			robotBelt[0] = true;
		}

		// 4. 내구도가 0인 칸의 개수가 k 개 이상이라면 과정 종료
		let temp = 0;
		for (let i = 0; i < n; i++) {
			if (upBelt[i] < 1) temp += 1;
			if (downBelt[i] < 1) temp += 1;
		}
		if (temp >= k) break;
	}

	return cnt;
};

console.log(solution(input));
```

---

## 개선한 방식의 문제 풀이 step 1

- 굳이 컨베이어 벨트를 위의 벨트와 아래의 벨트로 나눌 필요가 없어서 하나의 컨베이어 벨트로 구현했습니다.
  - 기존에는 회전을 구현하기 위해서 4 번의 함수 호출이 필요했으나, 개선한 방식에서는 2 번의 함수 호출로 해결이 되었습니다.
- 마지막에 컨베이어 벨트의 모든 요소를 확인하며 내구도가 0 인 부분을 찾는 방식으로 구현했으나, 그 대신에 내구도가 0 으로 변하는 그 부분에서만 검사하는 방식을 통해서 반복을 줄였습니다.
- 개선한 코드가 소요 시간이 덜 걸린 것을 확인할 수 있었습니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 개선한 방식의 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const [n, k] = input[0].split(" ").map(Number);
	const arr = input[1].split(" ").map(Number);
	const conveyorBelt = [];
	const robotBelt = Array(n).fill(false);

	for (let i = 0; i < n * 2; i++) conveyorBelt.push(arr[i]);

	let zero = 0;
	let cnt = 0;
	while (true) {
		cnt += 1;

		// 1. 벨트가 각 칸위에 있는 로봇과 함께 한 칸 회전
		// 개선한 부분 1. 함수의 호출 감소
		conveyorBelt.unshift(conveyorBelt.pop());
		robotBelt.pop();
		robotBelt.unshift(false);
		robotBelt[n - 1] = false;

		// 2. 가장 먼저 벨트에 올라간 로봇부터, 벨트가 회전하는 방향으로 한칸 이동 가능하면 이동
		for (let i = n - 1; i > 0; i--) {
			if (conveyorBelt[i] < 1) continue;
			if (robotBelt[i] === true) continue;
			if (robotBelt[i - 1] === false) continue;

			robotBelt[i] = true;
			robotBelt[i - 1] = false;

			conveyorBelt[i] -= 1;
			// 개선한 부분 2. 내구도가 변할 때 바로 검사
			if (conveyorBelt[i] === 0) zero += 1;
		}
		robotBelt[n - 1] = false;

		// 3. 올리는 위치에 있는 칸의 내구도가 0 이 아니면 올리는 위치에 로봇을 올린다.
		if (robotBelt[0] === false && conveyorBelt[0] > 0) {
			robotBelt[0] = true;
			conveyorBelt[0] -= 1;
			// 개선한 부분 2. 내구도가 변할 때 바로 검사
			if (conveyorBelt[0] === 0) zero += 1;
		}

		// 4. 내구도가 0인 칸의 개수가 k 개 이상이라면 과정 종료
		if (zero >= k) break;
	}

	return cnt;
};

console.log(solution(input));
```
