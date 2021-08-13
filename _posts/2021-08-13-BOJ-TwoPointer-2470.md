---
layout: post
title: "BOJ[2470] - 두 용액 by JavaScript"
date: 2021-08-13 12:00:00 +0900
categories: BOJ(TwoPointer)
---

# 두 용액

## 문제

- [백준 2470번 - 두 용액](https://www.acmicpc.net/problem/2470)

## 언어

- JavaScript

## 순서도

1. 주어진 용액 리스트를 특성값의 오름차순으로 정렬하기
2. left 와 right 두 포인터 선언
3. left 는 용액 리스트의 가장 왼쪽부터 순회, right 는 용액 리스트의 가장 오른뽁부터 순회
4. 순회하면서 left 와 right 의 용액 특성값의 합의 절대값의 최솟값을 갱신
5. 합한 특성값이 음수면 left 1 증가
6. 합한 특성값이 양수면 right 1 감소
7. 반복문이 종료되면 특성값의 합의 절대값이 최소일 때의 두 용액 출력

## 문제 풀이 step 1

- 두 포인터 형식의 문제로 두 개의 포인터를 선언하고 조작하며 문제에서 원하는 결과를 출력하는 문제입니다.
- 연구소에서 여러 종류의 용액을 보유하고 있습니다.
  - 각 용액은 특성값을 가지고 있습니다.
  - 연구소의 용액 중 임의의 두 개를 선택해서 특성값을 합했을 때 그 값의 절대값이 최소가 되는 경우의 두 용액을 찾는 문제입니다.
- 우선, 주어진 용액 리스트를 오름차순으로 정렬합니다.
- 정렬 후에 left 와 right 두 포인터를 선언합니다.
  - left 는 용액 리스트의 가장 왼쪽 용액을 가리킵니다. (최솟값)
  - right 는 용액 리스트의 가장 오른쪽 용액을 가리킵니다. (최댓값)
- left 와 right 가 가리키는 두 용액을 합하고, 최솟값과 비교하며 반복을 진행합니다.
  - 두 용액의 특성값의 합이 0 에 가장 가까운 경우를 찾아야해서 절대값 연산이 필요합니다.
  - 두 용액의 특성값의 합의 절대값이 최소라면 그 때의 두 용액을 기록합니다.
  - 그리고 두 용액의 특성값이 음수라면 left 포인터를 증가시키고, 양수라면 right 포인터를 감소시킵니다.
- 위 과정을 통해서 두 용액의 특성값의 합의 절대값이 가장 작은 경우의 두 용액을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 두 용액의 특성값의 차이가 아니라 합이여서 두 포인터 방식으로 풀 수 있었던 것 같습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const n = Number(input[0]);
	const liquid = input[1]
		.split(" ")
		.map(Number)
		.sort((a, b) => a - b);

	let min = Number.MAX_SAFE_INTEGER;
	let [leftLiquid, rightLiquid] = [0, 0];

	let [left, right] = [0, n - 1];
	while (left < right) {
		// 두 용액의 합 구하기
		let sum = liquid[left] + liquid[right];

		// 합의 절대값의 최소인 경우
		if (min > Math.abs(sum)) {
			// 최솟값 갱신
			min = Math.abs(sum);
			// 두 용액 갱신
			[leftLiquid, rightLiquid] = [liquid[left], liquid[right]];
		}

		// 합이 음수라면 left 포인터 증가
		if (sum <= 0) left += 1;
		// 합이 양수라면 right 포인터 감소
		else right -= 1;
	}

	return leftLiquid + " " + rightLiquid;
};

console.log(solution(input));
```
