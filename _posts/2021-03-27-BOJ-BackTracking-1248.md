---
layout: post
title: "BOJ[1248] - 맞춰봐 by JavaScript"
date: 2021-03-27 20:30:00 +0900
categories: BOJ(BackTracking)
---

# 맞춰봐

## 문제

- [백준 1248번 - 맞춰봐](https://www.acmicpc.net/problem/1248)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 백 트래킹 문제로 조건에 만족하면 반복을 종료함으로써, 반복의 수를 줄여서 시간 복잡도를 낮추는 알고리즘입니다.
- 문제를 잘 분석해보면 반복의 수를 줄일 수 있는 방법을 많이 찾을 수 있습니다. 하나씩 알아보겠습니다.
- 일단 문제가 상당히 길어서 문제 요약부터 하고 시작하겠습니다.
  - 우선 -10 ~ 10 까지 총 21 개의 수를 N 개 뽑는 문제입니다. 단, 해인이가 표현한 행렬 S 의 조건을 만족해야 합니다.
  - 해인이가 표현한 행렬 S 의 조건은 구간합들의 부호입니다. 즉, 부호가 + 이면 구간합이 양수여야 하고, 부호가 0 이면 구간합이 0, 부호가 - 이면 구간합이 음수여야 합니다.
- 문제의 설명은 여기까지 하고, 풀이를 알아보겠습니다.
- 모든 수열을 구하고, 구한 수열의 모든 구간합을 찾아서 해인이의 조건에 만족하는지 확인하는 방법이 있습니다.
- 하지만 이 문제는 백 트래킹 문제로 그렇게 하면 시간이 초과할 수도 있습니다. (시도는 안해봤지만, 범위가 커서 충분히 가능성이 있습니다.)
- 그래서 수열을 만드는 과정 속에서 해인이의 조건을 만족하지 않으면, 더이상 만들지 않음으로써 시간복잡도를 낮춰야합니다.
  - 그래서 수를 추가할 때마다 해인이의 조건에 만족하는지 확인해봐야 합니다.
  - 조건 비교는 간단하게 할 수 있습니다. 만약에 K 번째를 추가한 상황이라고 가정해보겠습니다.
  - 그럼 이미 1 ~ K - 1 번째 까지의 수들은 이미 해인이의 조건을 만족한 상황이고 저희는 K 번째 수만 조건 비교를 하면 됩니다.
    - 우선 S[K][k] 에 K 번째 수의 부호가 담겨 있으니 이를 비교합니다.
    - 그리고 S[K - 1][k] 에는 K - 1 번째 수와 K 번째 수의 합의 부호가 담겨 있습니다.
    - ...
    - S[0][k] 에는 0, 1, ..., K 번째 수들의 합의 부호가 담겨 있습니다.
    - 이렇게 해인이의 행렬 S 에서 위로만 올라가며 앞의 수들을 하나씩 합하면서 부호를 비교하면 됩니다.
- 그리고 정답을 하나라도 찾으면 반복을 종료하면 됩니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const checkLineSign = (depth, num, arr) => {
	let sum = 0;
	for (let i = depth - 1, x = depth - 1, y = depth - 1; i >= 0; i--, x--) {
		sum += num[i];

		if (arr[x][y] === "-" && sum >= 0) return false;
		else if (arr[x][y] === "+" && sum <= 0) return false;
		else if (arr[x][y] === "0" && sum !== 0) return false;
	}

	return true;
};

const rec = (n, arr, num, ans, depth) => {
	// 정답을 하나라도 찾으면 종료
	if (ans.length === 1) return;

	// 추가한 숫자가 해인이 만든 행렬의 조건에 안맞으면 재귀 종료
	if (depth > 0 && !checkLineSign(depth, num, arr)) return;

	if (depth === n) {
		ans.push(num.join(" "));
		return;
	}

	for (let i = -10; i <= 10; i++) {
		num.push(i);
		rec(n, arr, num, ans, depth + 1);

		num.pop();
	}
};

const solution = (input) => {
	const n = parseInt(input[0]);
	const arr = Array.from({length: n}, () => []);
	for (let i = 0, x = 0, y = 0; i < input[1].length; i++, y++) {
		const ch = input[1][i];
		if (y === n) {
			y = ++x;
		}
		arr[x][y] = ch;
	}

	const num = [];
	const ans = [];
	rec(n, arr, num, ans, 0);

	return ans[0];
};

console.log(solution(input));
```

---

## 다른 방식의 문제 풀이 step 1

- 또한 부호를 이용함으로써 반복을 1 ~ 10 으로 줄일 수 있는 방법이 있습니다.
- 저희는 해인의 행렬에서 수열의 각 숫자의 부호를 알 수 있습니다.
  - S[i][i] 에 각 숫자의 부호가 담겨 있습니다.
- 이를 이용해서 반복은 1 ~ 10 만하고, 부호를 붙여서 반복을 진행하면 반복을 조금이나마 줄일 수 있습니다.

## 소스코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const checkLineSign = (depth, num, arr) => {
	let sum = 0;
	for (let i = depth - 1, x = depth - 1, y = depth - 1; i >= 0; i--, x--) {
		sum += num[i];

		if (arr[x][y] === "-" && sum >= 0) return false;
		else if (arr[x][y] === "+" && sum <= 0) return false;
		else if (arr[x][y] === "0" && sum !== 0) return false;
	}

	return true;
};

const getSign = (arr, depth) => {
	if (arr[depth][depth] === "-") return -1;
	else if (arr[depth][depth] === "+") return 1;
	else return 0;
};

const rec = (n, arr, num, ans, depth) => {
	// 정답을 하나라도 찾으면 종료
	if (ans.length === 1) return;

	// 추가한 숫자가 해인이 만든 행렬의 조건에 안맞으면 재귀 종료
	if (depth > 0 && !checkLineSign(depth, num, arr)) return;

	if (depth === n) {
		ans.push(num.join(" "));
		return;
	}

	for (let i = 1; i <= 10; i++) {
		// 부호를 이용해 반복을 반으로 줄임
		const sign = getSign(arr, depth);

		num.push(i * sign);
		rec(n, arr, num, ans, depth + 1);

		num.pop();
	}
};

const solution = (input) => {
	const n = parseInt(input[0]);
	const arr = Array.from({length: n}, () => []);
	for (let i = 0, x = 0, y = 0; i < input[1].length; i++, y++) {
		const ch = input[1][i];
		if (y === n) {
			y = ++x;
		}
		arr[x][y] = ch;
	}

	const num = [];
	const ans = [];
	rec(n, arr, num, ans, 0);

	return ans[0];
};

console.log(solution(input));
```
