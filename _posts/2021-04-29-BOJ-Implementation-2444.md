---
layout: post
title: "BOJ[2444] - 별 찍기 - 7 by JavaScript"
date: 2021-04-29 09:45:00 +0900
categories: BOJ(Implementation)
---

# 별 찍기 - 7

## 문제

- [백준 2444번 - 별찍기 - 7](https://www.acmicpc.net/problem/2444)

## 언어

- JavaScript

## 순서도

1. N 번째 줄까지 양 사이드쪽의 별의 개수를 1 씩 늘리고, 가운데 공백의 개수를 2 씩 줄입니다.
2. 2 x N - 1 번째 줄까지 양 사이드쪽의 별의 개수를 1 씩 줄이고, 가운데 공백의 개수를 2 씩 늘립니다.

## 문제 풀이 step 1

- 문제에서 주어진 출력의 내용을 통해서 규칙을 파악하고, 규칙에 맞게 별을 출력하면 되는 문제입니다.
- 이번에는 기존의 별 찍기 문제 풀이와 조금 다르게 접근해봤습니다.
  - 첫 번째 줄에 공백과 별의 합의 개수가 N 입니다.
  - 그 이후로 N 번째 줄까지 공백과 별의 합의 개수가 1 씩 늘어나며, 최종 2 x N - 1 개가 됩니다.
    - 이 때 공백은 1 씩 줄어들고, 별은 둘(공백 + 별)의 합의 개수에서 공백의 수를 빼주면 됩니다.
  - 그 이후로 2 x N - 1 번째 줄까지 공백과 별의 합의 개수가 1 씩 줄어들며, 다시 N 개가 됩니다.
    - 이 때 공백은 1 씩 늘어나며, 별은 둘(공백 + 별)의 합의 개수에서 공백의 수를 빼주면 됩니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const getStars = (side, center) =>
	Array(side).fill(" ").concat(Array(center).fill("*")).join("") + "\n";

const solution = (input) => {
	let n = Number(input[0]);

	let res = "";

	let side = n;
	let center = 0;
	for (let i = n; i < 2 * n; i++) {
		side -= 1;
		center = i - side;
		res += getStars(side, center);
	}

	for (let i = 2 * n - 2; i >= n; i--) {
		side += 1;
		center = i - side;
		res += getStars(side, center);
	}

	return res;
};

console.log(solution(input));
```
