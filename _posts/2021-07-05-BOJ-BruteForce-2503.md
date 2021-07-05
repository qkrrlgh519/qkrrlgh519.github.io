---
layout: post
title: "BOJ[2503] - 숫자 야구 by JavaScript"
date: 2021-07-05 12:00:00 +0900
categories: BOJ(BruteForce)
---

# 숫자 야구

## 문제

- [백준 2503번 - 숫자 야구](https://www.acmicpc.net/problem/2503)

## 언어

- JavaScript

## 순서도

1. 1 에서 9 까지의 서로 다른 숫자 세 개로 만들 수 있는 모든 경우 만들기
2. 각 경우에 대해서 민혁이가 질문한 숫자를 적용했을 때, 영수의 대답과 똑같이 나오는지 비교
3. 영수의 대답과 똑같이 나올때만 답의 개수 증가시키기

## 문제 풀이 step 1

- 영수와 민혁이가 숫자 야구 게임을 합니다. 영수가 문제를 내고 민혁이가 맞혀야 합니다.
- 문제에서 영수와 민혁이가 주고받은 문답이 나옵니다. 그 문답을 이용해서 가능한 답의 개수를 출력하는 문제입니다.
- 브루트 포스 형식의 문제로 가능한 모든 경우를 비교해보는 방식으로 풀 수 있습니다.
- 가능한 모든 경우는 1 에서 9 까지의 서로 다른 숫자 세 개를 만드는 경우로 `9 x 8 x 7 = 504` 가지입니다.
- 따라서 충분히 구현 가능한 횟수입니다.

## 문제 풀이 step 2

- 우선, 1 에서 9 까지의 서로 다른 숫자 세 개를 모두 만듭니다.
  - 저는 재귀 방식을 사용했습니다. 대신에, 3 중 반복문을 쓰는 방식도 있습니다.
  ```jsx
  // 3 중 반복문
  for (let i = 0; i < 9; i++) {
  	for (let j = 0; j < 9; j++) {
  		if (j === i) continue;
  		for (let k = 0; k < 9; k++) {
  			if (k === i || k === j) continue;
  			// ijk 에 민혁이가 질문한 숫자를 적용했을 때, 영수의 대답과 똑같이 나오는지 비교
  		}
  	}
  }
  ```
- 각 경우를 영수가 속으로 생각한 수로 간주하고, 민혁이가 질문한 숫자를 적용해서 대답이 똑같이 나오는지 비교합니다.
- 만약 대답이 똑같이 나온다면 정답의 개수를 증가시키고, 똑같이 나오지 않는다면 정답의 개수를 증가시키지 않습니다.
- 위 과정을 통해서 나온 정답의 개수를 출력하면 정답이 됩니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드 1

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

let cnt = 0;

// 서로 다른 숫자 세 개가 영수와 민혁이가 주고 받은 문답과 동일한지 비교하는 함수
const check = (n, questions, arr) => {
	for (let i = 0; i < n; i++) {
		let [qNum, qStrike, qBall] = questions[i];
		qNum = qNum.split("").map(Number);
		qStrike = Number(qStrike);
		qBall = Number(qBall);

		let strike = 0;
		let ball = 0;
		for (let k = 0; k < 3; k++) {
			// 스트라이크의 개수 세기
			if (qNum[k] === arr[k]) strike += 1;
			else {
				for (let l = 0; l < 3; l++) {
					// 볼의 개수 세기
					if (qNum[l] === arr[k]) ball += 1;
				}
			}
		}

		// 서로 다를 숫자 세 개의 스트라이크와 볼의 개수가 영수의 대답과 동일하지 않은 경우
		if (strike !== qStrike || ball !== qBall) return false;
	}

	// 서로 다를 숫자 세 개의 스트라이크와 볼의 개수가 영수의 대답과 동일한 경우
	return true;
};

// 재귀 함수를 이용해서 1 ~ 9 까지의 서로 다른 숫자 세 개 만드는 함수
const rec = (n, questions, num, visited, arr, depth) => {
	if (depth === 3) {
		// 숫자 세 개가 영수와 민혁이가 주고 받은 문답과 동일한지 비교
		if (check(n, questions, arr) === true) cnt += 1;
		return;
	}

	for (let i = 0; i < 9; i++) {
		if (visited[i]) continue;

		visited[i] = true;
		arr.push(num[i]);
		rec(n, questions, num, visited, arr, depth + 1);

		visited[i] = false;
		arr.pop();
	}
};

const solution = (input) => {
	const n = Number(input[0]);
	const questions = input.slice(1, n + 1).map((v) => v.split(" "));

	const num = [1, 2, 3, 4, 5, 6, 7, 8, 9];
	const visited = Array(9).fill(false);
	const arr = [];
	rec(n, questions, num, visited, arr, 0);

	return cnt;
};

console.log(solution(input));
```
