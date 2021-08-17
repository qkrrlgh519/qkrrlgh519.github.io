---
layout: post
title: "BOJ[3273] - 두 수의 합 by JavaScript"
date: 2021-08-17 12:00:00 +0900
categories: BOJ(TwoPointer)
---

# 두 수의 합

## 문제

- [백준 3273번 - 두 수의 합](https://www.acmicpc.net/problem/3273)

## 언어

- JavaScript

## 순서도

1. 주어진 수열을 오름차순으로 정렬하기
2. left 와 right 두 포인터 선언
3. left 는 최솟값을 가리키도록, right 는 최댓값을 가리키도록 설정
4. 각 포인터들이 서로를 향해 한 칸씩 이동해가며, 두 포인터가 가리키는 수의 합이 x 인 경우 세기
5. 수의 합이 x 보다 작다면 left 포인터를 1 증가
6. 수의 합이 x 보다 크다면 right 포인터를 1 감소
7. 반복문이 종료되면 수의 합이 x 인 경우의 개수 출력

## 문제 풀이 step 1

- 두 포인터 형식의 문제로 두 개의 포인터를 선언하고 조작하며 문제에서 원하는 결과를 출력하는 문제입니다.
- n 개의 서로 다른 양의 정수로 이루어진 수열이 주어집니다. 수열에서 두 개의 수를 선택했을 때의 합이 x 가 되는 경우를 세는 문제입니다.
- 우선, 수열을 오름차순으로 정렬합니다.
  - 수열의 순서가 중요한 것이 아니기 때문에 정렬해서 풀어도 상관없습니다.
- 두 개의 포인터를 선언합니다.
  - left 포인터는 최솟값을 가리키고,
  - right 포인터는 최댓값을 가리킵니다.
- 반복을 진행하며, 두 개의 포인터가 가리키는 수들의 합이 x 가 되는 경우의 개수를 셉니다.
  - 수들의 합이 x 보다 작다면, left 포인터를 1 증가시킵니다.
  - 수들의 합이 x 보다 크다면, right 포인터를 1 감소시킵니다.
- 두 포인터가 만나는 순간, 반복을 종료하고 경우의 개수를 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 아직 두 포인터 유형의 문제에 익숙하진 않지만, 두 포인터를 조작하는 방법에는 몇 가지 유형이 있는 것 같습니다.
  - (부분합) left 와 right 둘 다 작은 값에서 1 씩 증가하는 유형
  - left 와 right 가 끝과 끝에서 서로를 향해 1 씩 이동하는 유형
  - 완전 다른 배열에서 두 포인터가 각각 1 씩 증가하는 유형

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const n = Number(input[0]);
	// 오름차순 정렬
	const nums = input[1]
		.split(" ")
		.map(Number)
		.sort((a, b) => a - b);
	const x = Number(input[2]);

	// n 이 1 인 경우는 쌍이 존재할 수 없음
	if (n === 1) return 0;

	let cnt = 0;
	// 두 포인터 생성
	let [left, right] = [0, n - 1];
	while (left < right) {
		const sum = nums[left] + nums[right];

		if (sum <= x) {
			// 합이 x 인 경우
			if (sum === x) cnt += 1;
			// 합이 x 보다 작은 경우
			left += 1;
		} else {
			// 합이 x 보다 큰 경우
			right -= 1;
		}
	}

	return cnt;
};

console.log(solution(input));
```
