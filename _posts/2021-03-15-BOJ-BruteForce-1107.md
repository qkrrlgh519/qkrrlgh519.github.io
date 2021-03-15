---
layout: post
title: "BOJ[1107] - 리모컨 by JavaScript"
date: 2021-03-15 23:20:00 +0900
categories: BOJ(BruteForce)
---

# 리모컨

## 문제

- [백준 1107번 - 리모컨](https://www.acmicpc.net/problem/1107)

## 언어

- JavaScript

## 문제 풀이 step 1

- 완전 탐색 문제로 모든 경우의 수를 고려해서 풀면 됩니다.
- 범위는 0 ~ 999999 로 충분히 가능한 범위입니다.
- 범위는 이동하려고 하는 채널 N의 최대 크기가 500000 이므로 자릿수만큼 9번 버튼을 누른 999999 로 정할 수 있습니다.
- 채널 N으로 이동하기 위한 방법은 2가지 입니다.
  - 현재 채널 100 에서 위, 아래 버튼을 눌러서 채널 N으로 이동하는 방법
    - 횟수 = Math.abs(n - 100)
  - 0 ~ 999999 채널 중에서 고장나지 않은 버튼으로 만들 수 있는 채널에서 위, 아래 버튼을 눌러서 채널 N으로 이동하는 방법
    - 횟수 = Math.abs(n - (0 ~ 999999)) + (0 ~ 999999)의 자릿수 (단, 고장나지 않은 버튼으로 만들 수 있는 수)
- 위의 두 방법을 만들 수 있는 모든 경우 중에서 최소값이 정답이 됩니다.

## 추가로 배운 점

- 고장난 채널의 개수가 0 일 수도 있는데 `for(let i = 0; i < inputArr.length; i++)` 처럼 대충 코딩할 경우 오류가 발생합니다.
- 주어진 변수 `m`을 이용하거나, 좀 더 빡빡하게 생각해서 코딩을 한다면 이런 오류를 피할 수 있습니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const solution = (input) => {
	const n = parseInt(input[0]);
	const m = parseInt(input[1]);
	const inptuArr = input[2].split(" ").map(Number);

	// 망가지지 않은 버튼들
	const brokens = Array(10).fill(false);
	for (let i = 0; i < m; i++) {
		brokens[inptuArr[i]] = true;
	}

	// 1. 채널 100에서 이동하는 방식
	let ans = Math.abs(100 - parseInt(n));

	// 1.5 0 예외 처리
	if (!brokens[0]) ans = Math.min(ans, n + 1);

	// 2. 채널 변경 후 이동하는 방식
	for (let i = 1; i < 1000000; i++) {
		let possible = true;
		let num = i;
		let numLength = 0;

		// 각 자릿수가 고장난 버튼인지 아닌지 확인
		while (num !== 0) {
			if (brokens[num % 10]) {
				possible = false;
				break;
			}

			num = parseInt(num / 10);
			numLength++;
		}

		if (!possible) continue;

		ans = Math.min(ans, Math.abs(n - i) + numLength);
	}

	return ans;
};

console.log(solution(input));
```
