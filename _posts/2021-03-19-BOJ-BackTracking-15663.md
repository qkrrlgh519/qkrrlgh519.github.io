---
layout: post
title: "BOJ[15663] - N과 M (9) by JavaScript"
date: 2021-03-19 21:40:00 +0900
categories: BOJ(BackTracking)
---

# N과 M (9)

## 문제

- [백준 15663번 - N과 M (9)](https://www.acmicpc.net/problem/15663)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 백 트래킹 문제로 조건에 만족하면 반복을 종료함으로써, 반복의 수를 줄여서 시간 복잡도를 낮추는 알고리즘입니다.
- N 개의 자연수 중에서 M 개를 골라야 합니다.
- 주어진 N 개의 자연수에 같은 수가 여러 개 존재할 수 있으나, 고른 수열은 중복되면 안됩니다.
- 그래서 방문 처리를 하면서 M 번 만큼의 탐색하는 모든 경우의 수를 구하고, Set 을 이용해서 중복을 제거합니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const rec = (n, m, arr, res, step, visited, depth) => {
	if (depth === m) {
		res.push(step.join(" "));
		return;
	}

	for (let i = 0; i < n; i++) {
		if (visited[i]) continue;

		visited[i] = true;
		step.push(arr[i]);
		rec(n, m, arr, res, step, visited, depth + 1);

		visited[i] = false;
		step.pop();
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	const arr = input[1]
		.split(" ")
		.map(Number)
		.sort((a, b) => a - b);

	const res = [];
	const step = [];
	const visited = Array(n).fill(false);
	rec(n, m, arr, res, step, visited, 0);

	// 중복 제거
	return Array.from(new Set(res)).join("\n");
};

console.log(solution(input));
```

---

## 다른 방식의 문제 풀이 step 1

- 위의 방식은 구현하기 쉽지만, 마지막에 중복를 제거 하는 과정 때문에 시간이 더 오래 걸립니다.
- 그런 부분을 개선할 수 있었으면 좋겠습니다.
- 아래의 방식은 수의 종류의 개수 만큼만 탐색을 하기에, 별도로 중복을 제거하는 로직이 필요 없습니다.
- 구현 방식에 대해서 알아보자면,
- 우선 수의 종류의 개수(num)를 셉니다. 그리고 각 종류마다 몇 개(count)씩 존재하는지 파악합니다.
- 그리고 탐색은 수의 종류의 개수(num)만큼 진행합니다. 중복 체크는 선택한 종류의 개수(count)를 줄이는 방식으로 진행합니다.
- 마찬가지로 탐색은 m 번의 깊이만큼 진행하면 종료합니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const rec = (n, m, arr, res, step, num, count, depth) => {
	if (depth === m) {
		res.push(step.join(" "));
		return;
	}

	for (let i = 0; i < num.length; i++) {
		if (count[i] === 0) continue;

		count[i] -= 1;
		step.push(num[i]);
		rec(n, m, arr, res, step, num, count, depth + 1);

		count[i] += 1;
		step.pop();
	}
};

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);
	const arr = input[1]
		.split(" ")
		.map(Number)
		.sort((a, b) => a - b);

	// 주어진 숫자들의 종류와 각 종류의 개수 세기
	const temp = {};
	for (let i = 0; i < n; i++) {
		temp[arr[i]] ? temp[arr[i]]++ : (temp[arr[i]] = 1);
	}

	// 객체를 key와 value로 나눠서 배열화
	const num = Object.keys(temp);
	const count = Object.values(temp);

	const res = [];
	const step = [];
	rec(n, m, arr, res, step, num, count, 0);

	return Array.from(new Set(res)).join("\n");
};

console.log(solution(input));
```
