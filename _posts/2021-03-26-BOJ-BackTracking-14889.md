---
layout: post
title: "BOJ[14889] - 스타트와 링크 by JavaScript"
date: 2021-03-26 13:00:00 +0900
categories: BOJ(BackTracking)
---

# 스타트와 링크

## 문제

- [백준 14889번 - 스타트와 링크](https://www.acmicpc.net/problem/14889)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 백 트래킹 문제로 조건에 만족하면 반복을 종료함으로써, 반복의 수를 줄여서 시간 복잡도를 낮추는 알고리즘입니다.
- 문제를 잘 분석해보면 반복의 수를 줄일 수 있는 방법을 많이 찾을 수 있습니다. 하나씩 알아보겠습니다.
- 우선 문제를 보면 스타트 팀과 링크 팀이 있습니다. 이 부분에서 잘 생각해보면 스타트 팀만 구하면 저절로 링크 팀이 구해진다는 사실을 알 수 있습니다.
- 즉, 선수가 4 명이고 1, 2, 3, 4 이렇게 되어있을 때 스타트 팀을 1, 2 이렇게 구하면, 저절로 링크 팀은 3, 4 이렇게 됩니다.
- 그래서 저희는 4 명 중에서 2 명만 선택하는 알고리즘을 사용할 수 있겠습니다.
  - 4 명 중에서 2 명만 구하는 방법을 나열해보겠습니다.
  - [1, 2], [1, 3], [1, 4], [2, 1], [2, 3], [2, 4], [3, 1], [3, 2], [3, 4], [4, 1], [4, 2], [4, 3]
  - 잘 보면 앞의 값들이 뒤의 값들과 중복된다는 것을 확인할 수 있습니다.
  - **[반복을 줄이는 방법 1]** 예를 들어 [1, 2] 와 [2, 1] 은 같은 경우겠죠. 이렇게 같은 경우들을 제외하고 나열해보겠습니다.
  - [1, 2], [1, 3], [1, 4], [2, 3], [2, 4], [3, 4]
  - 즉, 중복 없이, 오름 차순으로 선택하는 방법으로 나온 결과와 동일한 것을 확인할 수 있습니다.

## 문제 풀이 step 2

- 위의 상태에서 한번 더 반복을 줄일 수 있습니다.
- 위에서 구한 [1, 2], [1, 3], [1, 4], [2, 3], [2, 4], [3, 4] 는 스타트 팀을 나타낸 것입니다. 각 팀들을 링크 팀과 연결시켜 주겠습니다.
  - [1, 2], [3, 4]
  - [1, 3], [2, 4]
  - [1, 4], [2, 3]
  - [2, 3], [1, 4]
  - [2, 4], [1, 3]
  - [3, 4], [1, 2]
  - 이렇게 연결시켜 보니 중복된 것이 보이네요.
  - 저희는 결국 스타트 팀과 링크 팀의 점수의 차이를 구해야 하기 때문에 그들을 중복된 것으로 간주할 수 있습니다.
  - **[반복을 줄이는 방법 2]** 즉, [1, 2], [3, 4]와 [3, 4], [1, 2]가 같은 경우라는 것이죠. 이렇게 같은 경우들을 제외하고 나열해보겠습니다.
    - [1, 2], [3, 4]
    - [1, 3], [2, 4]
    - [1, 4], [2, 3]
    - 결국 첫 번째 선수를 스타트 팀에 포함한 경우만 계산해보면 되겠습니다.
- 이렇게 구한 각 경우들의 점수 차이 중에서 최솟값을 구하면 정답이 됩니다.

## 후기

- 결국 BruteForce 에서 모든 경우를 탐색하지 않고, 의미 없는 탐색은 종료하는 것이 BackTracking 인데요.
- 어떤 문제를 만나고 어! 이건 BackTracking 문제네 라고 떠오르면 좋겠지만 그것이 안될 수도 있을 것 같습니다.
- 그래서 매 문제를 만날 때마다 시간을 줄일 방법을 생각하면서 한다면 저절로 BackTacking 기법으로 풀게 되지 않을까 라는 생각을 하게되었습니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

let min = Infinity;

// boolean 배열 형태로 팀을 나눠서 그것을 다시 실제 배열로 나누는 함수
const splitTeam = (selected) => {
	const start = [];
	const link = [];

	for (let i = 0; i < selected.length; i++) {
		if (selected[i]) start.push(i);
		else link.push(i);
	}

	return [start, link];
};

// 팀의 점수를 구하는 함수
const calculate = (s, team) => {
	let point = 0;
	for (let i = 0; i < team.length; i++) {
		for (let j = 0; j < team.length; j++) {
			if (i === j) continue;
			point += s[team[i]][team[j]];
		}
	}

	return point;
};

const rec = (n, s, selected, index, depth) => {
	// 첫 번재 선수가 포함되지 않는 경우는 재귀 종료
	if (depth > 0 && !selected[0]) return;

	if (depth === n / 2) {
		const [start, link] = splitTeam(selected);

		let startPoint = calculate(s, start);
		let linkPoint = calculate(s, link);

		min = Math.min(min, Math.abs(startPoint - linkPoint));
		return;
	}

	if (index >= n) return;

	selected[index] = true;
	rec(n, s, selected, index + 1, depth + 1);

	selected[index] = false;
	rec(n, s, selected, index + 1, depth);
};

const solution = (input) => {
	const n = parseInt(input[0]);
	const s = input.slice(1).map((v) => v.split(" ").map(Number));

	const selected = Array(n).fill(false);
	rec(n, s, selected, 0, 0);

	return min;
};

console.log(solution(input));
```
