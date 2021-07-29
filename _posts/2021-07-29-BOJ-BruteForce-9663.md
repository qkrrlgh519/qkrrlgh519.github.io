---
layout: post
title: "BOJ[9663] - N-Queen by JavaScript"
date: 2021-07-29 13:00:00 +0900
categories: BOJ(BruteForce)
---

# N-Queen

## 문제

- [백준 9663번 - N-Queen](https://www.acmicpc.net/problem/9663)

## 언어

- JavaScript

## 순서도

1. N x N 크기의 체스판에서 퀸들이 서로 공격할 수 없게 각 행마다 퀸을 1 개씩 놓을 수 있는 모든 경우 만들기
2. 그 경우의 개수 세서 출력하기

## 문제 풀이 step 1

- 본 문제는 크기가 N x N 인 체스판 위에 퀸 N 개를 서로 공격할 수 없게 놓는 방법의 수를 구하는 문제입니다.
- 우선, 퀸은 체스에서 가장 중요한 말로 총 8 가지 방향으로 공격이 가능합니다.
  - 동, 서, 남, 북
  - 북동, 북서, 동남, 남서
- 이러한 퀸의 공격 방향을 토대로 퀸을 놓는 위치에 대한 규칙을 몇 가지 찾아볼 수 있었습니다.
  - 퀸은 한 열에 한 개만 놓을 수 있습니다.
  - 퀸은 한 행에 한 개만 놓을 수 있습니다.
  - 퀸은 북동, 남서 방향의 대각선에 한 개만 놓을 수 있습니다.
  - 퀸은 북서, 동남 방향의 대각선에 한 개만 놓을 수 있습니다.

## 문제 풀이 step 2

- 재귀 함수를 이용해서 위에서 알아본 규칙을 잘 지키며 퀸을 놓을 수 있는 모든 방법의 수를 세어주고 출력하면 정답입니다.
- 재귀함수는 총 5 가지의 매개 변수를 받습니다. (n, column, leftDiagonal, rightDiagonal, depth)
  - n 은 체스판의 행과 열의 크기를 의미합니다.
  - depth 는 행의 번호를 의미하는 변수로, 매 호출마다 1 씩 증가시켜서 한 행에 하나의 퀸만 놓을 수 있도록 해줍니다.
  - column 은 1 차원 배열로 각 열을 의미하며, 매 호출마다 퀸을 놓은 열을 기록해서 한 열에 하나의 퀸만 놓을 수 있도록 해줍니다.
  - leftDiagonal 은 N x N 크기의 체스판에서 왼쪽에서 오른쪽으로 내려오는 각각의 대각선에 하나의 퀸만 놓을 수 있도록 해줍니다.
  - rightDiagonal 은 N x N 크기의 체스판에서 왼쪽에서 오른쪽으로 올라가는 각각의 대각선에 하나의 퀸만 놓을 수 있도록 해줍니다.
- 재귀함수를 매번 호출할 때마다 위의 매개변수들을 이용해서 놓으려는 퀸의 위치가 다른 퀸을 공격하지 않는지 검사해주면서 N 개의 퀸을 놓고, 그 방법의 수를 출력하면 정답입니다.

## 문제 풀이 step 3

- 한 행에 하나의 퀸만, 한 열에 하나의 퀸만 놓는 것을 검사하는 것은 크게 어렵지 않았습니다.
  - depth 변수를 이용해서 매 호출마다 1 씩 증가시켜주면 되기 때문입니다.
  - column 배열을 이용해서 매 호출마다 j 좌표에 해당하는 인자의 값을 바꿔주면 되기 때문입니다.
- 좌 대각선과 우 대각선을 검사하는 것이 조금 어려웠는데요. 그 부분만 알아보고 넘어가겠습니다.
- 좌 대각선 : 왼쪽에서 오른쪽으로 내려오는 대각선
  ```jsx
  const arr = [
  	"[0, 0], [0, 1], [0, 2]",
  	"[1, 0], [1, 1], [1, 2]",
  	"[2, 0], [2, 1], [2, 2]",
  ];
  ```
  - 대각선은 총 5 개 있습니다.
    - `[2, 0]`
    - `[1, 0], [2, 1]`
    - `[0, 0], [1, 1], [2, 2]`
    - `[0, 1], [1, 2]`
    - `[0, 2]`
  - 대각선 배열의 크기는 `2 * N - 1` 입니다.
  - 이러한 대각선의 좌표들을 1 차원 배열 leftDiagonal 의 좌표로 매핑시키기 위한 규칙은
  - y 좌표에서 x 좌표를 빼보면 됩니다.
    - `- 2`
    - `- 1`
    - `0`
    - `1`
    - `2`
  - 결과값들이 오름차순으로 되어있는 것을 확인할 수 있습니다.
  - 그리고 각 결과값에 `N -1` 을 더해주면 leftDiagonal 의 좌표로 매핑시킬 수 있게 됩니다.
  - 이런 규칙과 leftDiagonal 을 이용해서 왼쪽에서 오른쪽으로 내려오는 대각선마다 하나의 퀸만 놓을 수 있게 검사합니다.

## 문제 풀이 step 4

- 우 대각선 : 왼쪽에서 오른쪽으로 올라가는 대각선
  ```jsx
  const arr = [
  	"[0, 0], [0, 1], [0, 2]",
  	"[1, 0], [1, 1], [1, 2]",
  	"[2, 0], [2, 1], [2, 2]",
  ];
  ```
  - 마찬가지로 대각선은 총 5 개 있습니다.
    - `[0, 0]`
    - `[1, 0], [0, 1]`
    - `[2, 0], [1, 1], [0, 2]`
    - `[2, 1], [1, 2]`
    - `[2, 2]`
  - 마찬가지로 대각선 배열의 크기는 `2 * N - 1` 입니다.
  - 이러한 대각선의 좌표들을 1 차원 배열 rightDiagonal 의 좌표로 매핑시키기 위한 규칙은
  - x 좌표와 y 좌표를 더해주면 됩니다.
    - `0`
    - `1`
    - `2`
    - `3`
    - `4`
  - 결과값들을 보면 rightDiagonal 의 좌표로 매핑시킬 수 있는 형태를 띄고 있습니다.
  - 이런 규칙과 rightDiagonal 을 이용해서 왼쪽에서 오른쪽으로 올라가는 대각선마다 하나의 퀸만 놓을 수 있게 검사합니다.
- 위 부분 말고는 크게 신경쓸 부분은 없고, 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 본 문제는 구현 방법에 따라서 소요시간이 상당히 많이 달라집니다.
  - 2 차원 배열을 이용해서 매번 퀸을 놓으려는 위치에서 반복을 돌면서 8 방향을 검사하는 방식은 시간이 상당히 오래 걸립니다. (이 경우, 약 16396 ms 가 소요)
  - 위에서 구현한 방법대로 행 넘어가기, 열 검사, 좌 대각선 검사, 우 대각선 검사를 해주면 상당히 많은 시간을 감소시킬 수 있습니다. (이 경우, 약 2944 ms 가 소요)
- 처음에 풀었던 정답이 소요시간이 너무 오래걸려서 충격을 먹은 나머지 어떻게든 소요시간을 줄여보려고 이것 저것 다 해봤는데 그 과정 속에서 많은 공부를 할 수 있었습니다.

## 소스 코드 1

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

let ans = 0;

const rec = (n, column, leftDiagonal, rightDiagonal, depth) => {
	if (depth === n) {
		ans += 1;
		return;
	}

	for (let j = 0; j < n; j++) {
		// 열 검사
		if (column[j] === true) continue;
		// 좌 대각선 검사
		if (leftDiagonal[n - 1 - (depth - j)] === true) continue;
		// 우 대각선 검사
		if (rightDiagonal[depth + j] === true) continue;

		column[j] = true;
		leftDiagonal[n - 1 - (depth - j)] = true;
		rightDiagonal[depth + j] = true;
		// depth + 1 을 통해서 행 넘어가기
		rec(n, column, leftDiagonal, rightDiagonal, depth + 1);

		column[j] = false;
		leftDiagonal[n - 1 - (depth - j)] = false;
		rightDiagonal[depth + j] = false;
	}
};

const solution = (input) => {
	const n = Number(input[0]);

	const column = Array.from({length: n}, () => false);
	const leftDiagonal = Array.from({length: 2 * n - 1}, () => false);
	const rightDiagonal = Array.from({length: 2 * n - 1}, () => false);

	rec(n, column, leftDiagonal, rightDiagonal, 0);

	return ans;
};

console.log(solution(input));
```
