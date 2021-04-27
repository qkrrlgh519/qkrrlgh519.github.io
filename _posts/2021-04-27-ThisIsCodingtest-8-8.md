---
layout: post
title: "이것이 코딩테스트다 - [DynamicProgramming] 효율적인 화폐 구성 by JavaScript"
date: 2021-04-27 13:30:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 효율적인 화폐 구성

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- **점화식**
  - dp[N] = N 원을 만들기 위한 최소한의 화폐 개수
- **경우**
  - 화폐의 종류에 따라 달라집니다. 예를 들어 화폐가 2, 3, 5 가 주어진다면,
  1.  N - 2 원을 만들기 위한 최소한의 화폐 개수에 2 원을 추가하는 경우
  2.  N - 3 원을 만들기 위한 최소한의 화폐 개수에 3 원을 추가하는 경우
  3.  N - 5 원을 만들기 위한 최소한의 화폐 개수에 5 원을 추가하는 경우
  - 최소한의 화폐 개수이니 3 가지의 경우 중에서 최솟값이 dp[N] 이 됩니다.

## 후기 1

- 다시 풀어볼 문제입니다.
- **N 원을 주어진 화폐로 만들 수 없는 경우**와 **N 원이 주어진 화폐보다 작은 경우**, 이 2 가지 경우를 고려해야 해서 추가적인 로직이 필요한 문제였습니다.
- 처음에는 dp 배열을 초기화하지 않고, 구현하려다 보니 조건이 복잡해지고, 로직이 더러워졌습니다.
- 그래서 우선 dp 배열을 -1 로 초기화 하고 구현하니 조건을 약간 간소화할 수 있었습니다.
- 아래와 같은 조건을 만들 수 있었습니다.
  1. N 원이 화폐보다 작은 경우, (N - 화폐) 원이 이전 과정에서 만들 수 없다고 판별이 난 경우
     - 만들 수 없는 경우이므로 그냥 넘어갑니다. (어차피 -1 로 초기화했기 때문에)
  2. 1 번 이외의 경우,
     1. -1 로 초기화한 이래로 값이 갱신되지 않은 경우
     2. 값이 한 번이라도 갱신된 경우

## 후기 2

- 저자님께서는 **dp 배열을 -1 이 아닌 MAX 값 (10001) 으로 초기화**하고, 못 만드는 N 원에 대해서도 MAX 값 (10001) 으로 설정하셔서 로직이 더욱 간결합니다.
  - 로직을 구현하면서 사용할 함수가 **`Math.min(a, b)`** 이기 때문에 MAX 값 으로 초기화하면 조건을 더욱 간결하게 만들 수 있는 것 같습니다.
- 그리고 반복문의 순서를 다르게 해서 N 원이 화폐보다 작은 경우를 아예 배제하셨습니다. 이로써 더 조건도 간결해지고, 로직도 깔끔해집니다.
- 제 방식은 1 원 부터 N 원까지 1 원씩 늘려가며 각 돈에 대해서 주어진 화페로 경우를 비교하며 dp 배열을 채워갑니다.
- 반면에 저자님 방식은 주어진 화폐를 바꿔가며 각 화폐마다 N 원을 만들 수 있다면, 개수를 계산하는 방식으로 dp 배열을 채워갑니다.
- 저자님의 방식대로 하면 고려해야 할 조건도 줄고, 로직도 더욱 깔끔해집니다.

## 소스 코드 (블로거)

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `3 4
// 3
// 5
// 7`.split("\n");

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);

	const dp = [0];
	const coins = [];
	for (let i = 1; i <= n; i++) {
		coins[i - 1] = Number(input[i]);
		dp[coins[i - 1]] = 1;
	}

	for (let i = 1; i <= m; i++) {
		if (dp[i] === undefined) dp[i] = -1;

		for (let j = 0; j < n; j++) {
			const coin = coins[j];
			if (i - coin < 0 || dp[i - coin] === -1) continue;

			dp[i] === -1
				? (dp[i] = dp[i - coin] + 1)
				: (dp[i] = Math.min(dp[i], dp[i - coin] + 1));
		}
	}

	return dp[m];
};

console.log(solution(input));
```

## 소스 코드 (저자님)

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `2 15
// 2
// 3`.split("\n");

const MAX = 10001;

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);

	const dp = Array(m + 1).fill(MAX);
	dp[0] = 0;
	const coins = [];
	for (let i = 1; i <= n; i++) {
		coins[i - 1] = Number(input[i]);
	}

	for (let i = 0; i < n; i++) {
		const coin = coins[i];
		for (let j = coin; j <= m; j++) {
			if (dp[j - coin] !== MAX) dp[j] = Math.min(dp[j], dp[j - coin] + 1);
		}
	}

	if (dp[m] === MAX) return -1;
	else return dp[m];
};

console.log(solution(input));
```
