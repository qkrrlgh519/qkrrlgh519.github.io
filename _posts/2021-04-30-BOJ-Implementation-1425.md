---
layout: post
title: "BOJ[1475] - 방 번호 by JavaScript"
date: 2021-04-30 23:00:00 +0900
categories: BOJ(Implementation)
---

# 방 번호

## 문제

- [백준 1475번 - 방 번호](https://www.acmicpc.net/problem/1475)

## 언어

- JavaScript

## 순서도

1. 방 번호의 개수 세기 (이 때, 6 번과 9 번은 함께 세기)
2. 최소로 필요한 세트의 개수 계산하기

## 문제 풀이 step 1

- 필요한 세트의 개수를 계산해서 출력하는 문제입니다.
- 이 때, 6 번과 9 번은 서로 뒤집어서 사용할 수 있기 때문에 같은 종류라고 봐도 무방합니다. 따라서 같이 세줍니다.
- 그리고 한 세트에 6 번과 9 번 각각 한 개씩 있을텐데, 둘을 같은 종류라고 본다면 같은 종류의 플라스틱 숫자가 2 개라고 볼 수 있습니다.
- 그래서 문제를 풀 때 간편하게 생각해서 9 라는 숫자는 없다고 생각하고 6 이 2 개라고 생각하고 풀면 쉽게 풀 수 있습니다.
  - 즉, `한 세트 = [0, 1, 2, 3, 4, 5, 6, 6, 7, 8]` 이렇게 볼 수 있는 것입니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const solution = (input) => {
	const n = input[0].split("").map(Number);

	let max = 0;
	const arr = Array(9).fill(0);

	// 1. 방 번호의 개수 세기 (이 때, 6 번과 9 번은 함께 세기)
	for (let i = 0; i < n.length; i++) {
		if (n[i] === 9) arr[6] += 1;
		else arr[n[i]] += 1;

		if (n[i] !== 6 && n[i] !== 9) {
			max = Math.max(max, arr[n[i]]);
		}
	}

	// 2. 최소로 필요한 세트의 개수 계산하기
	return Math.max(max, Math.round(arr[6] / 2));
};

console.log(solution(input));
```
