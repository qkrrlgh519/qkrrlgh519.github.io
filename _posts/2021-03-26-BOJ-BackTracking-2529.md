---
layout: post
title: "BOJ[2529] - 부등호 by JavaScript"
date: 2021-03-26 17:00:00 +0900
categories: BOJ(BackTracking)
---

# 부등호

## 문제

- [백준 2529번 - 부등호](https://www.acmicpc.net/problem/2529)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 백 트래킹 문제로 조건에 만족하면 반복을 종료함으로써, 반복의 수를 줄여서 시간 복잡도를 낮추는 알고리즘입니다.
- 문제를 잘 분석해보면 반복의 수를 줄일 수 있는 방법을 많이 찾을 수 있습니다. 하나씩 알아보겠습니다.
- 우선 문제를 보면 0 ~ 9 이렇게 10 개의 수 중에서 부등호의 개수보다 1개 더 많이 선택한 후, 선택한 수들을 부등호 사이 사이에 넣어서 부등호 관계를 만족하는 수열을 찾아야 합니다.
- 문제를 보면 정답은 조건을 만족하는 수열들 중에서 최댓값과 최솟값을 찾는 것입니다.
- **[반복을 줄이는 방법 1]** 즉, 조건을 만족하는 모든 수열을 구할 필요가 없습니다. 그저 순서가 낮은 수열부터 올라가면서 최솟값을 찾으면 탐색을 종료하고, 순서가 높은 수열부터 내려오면서 최댓값을 찾으면 탐색을 종료하는 방식으로 정답을 구해도 됩니다.
  - 저는 아래와 같은 방법으로 반복을 줄였습니다.
  - `rec(arr, k, a, visited, res, number, 0);` : 배열을 순서대로 입력
  - `rec(arr.reverse(), k, a, visited, res, number, 0);` : 배열을 역순으로 입력
  - `if (res.length === 1) return;` : 조건을 만족하는 수를 하나만 찾으면 종료

## 문제 풀이 step 2

- **[반복을 줄이는 방법 2]** 수열의 각 수를 고를 때마다 부등호 관계를 만족하는 지 판별하면 굳이 불필요한 탐색을 할 필요가 없습니다.
- `if (depth > 1 && !check(a, number)) return;` : 선택한 수가 2개 이상이면 부등호 관계를 만족하는지 판별한다.
- 이러한 코드가 백트래킹 기법이 요구하는 방식일 것입니다.
- 굳이 수열을 완성한 다음에 마지막에 부등호 관계를 만족하는지 판별하지 말고, 중간에 부등호 관계를 만족하지 않으면 탐색을 종료하는 방식으로 구현하면 탐색을 줄일 수 있습니다.

## 시행 착오 (삽질)

- 처음에는 최솟값과 최댓값을 구하는 함수 2 개를 구현했습니다.
- 정말 안좋았던 것은 두 함수의 코드가 똑같고 중간에 반복문의 식만 달랐습니다.
  - getMin : `for (let i = 0; i < 10; i++) {`
  - getMax : `for (let i = 9; i >= 0; i--) {`
- 이 부분을 고쳐보고자 고심한 끝에 `const arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];` 형태의 배열을 생성하고, 순서대로 그리고 역순으로 넣어주는 방식으로 해결할 수 있었습니다.
  - getMin : `rec(arr, k, a, visited, res, number, 0);`
  - getMax : `rec(arr.reverse(), k, a, visited, res, number, 0);`
- 항상 느끼지만 문제를 해결하는 데 있어 이런 방식의 접근은 간단해보이지만 중요한 것 같습니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const check = (a, number) => {
	let index = number.length - 1;
	if (a[index - 1] === "<" && number[index - 1] > number[index]) return false;
	if (a[index - 1] === ">" && number[index - 1] < number[index]) return false;
	return true;
};

const rec = (arr, k, a, visited, res, number, depth) => {
	// 조건을 만족하는 수열 하나만 찾아도 바로 탐색 종료
	if (res.length === 1) return;

	// 수를 선택할 때마다 부등호 관계를 만족하는지 판별
	if (depth > 1 && !check(a, number)) return;

	if (depth === k + 1) {
		res.push(number.join(""));
		return;
	}

	for (let i = 0; i < 10; i++) {
		if (visited[i]) continue;

		visited[i] = true;
		number.push(arr[i]);
		rec(arr, k, a, visited, res, number, depth + 1);
		visited[i] = false;
		number.pop();
	}
};

const solution = (input) => {
	const k = parseInt(input[0]);
	const a = input[1].split(" ");
	const arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

	const res = [];
	const number = [];
	const visited = Array(10).fill(false);
	rec(arr, k, a, visited, res, number, 0);
	const min = res.pop();

	rec(arr.reverse(), k, a, visited, res, number, 0);
	const max = res.pop();

	return `${max}\n${min}`;
};

console.log(solution(input));
```
