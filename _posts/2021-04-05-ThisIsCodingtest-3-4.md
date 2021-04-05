---
layout: post
title: "이것이 코딩테스트다 - [Greedy] 1이 될 때까지 by JavaScript"
date: 2021-04-05 13:00:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 1이 될 때까지

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 본 문제는 그리디 알고리즘의 방식으로 풀 수 있습니다.
- 주어진 N 을 1 로 만들어야 합니다.
- 그 과정 속에서 1 을 빼는 방법보다는 최대한 K 로 나누는 방법을 많이 사용하는 방식으로 접근하면 정답을 구할 수 있습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `25 5`.split("\n");

const solution = (input) => {
	let [n, k] = input[0].split(" ").map(Number);

	let ans = 0;
	while (n !== 1) {
		if (n % k === 0) {
			n /= k;
		} else {
			n -= 1;
		}

		ans += 1;
	}

	return ans;
};

console.log(solution(input));
```

---

## 다른 방식의 문제 풀이 step 1

- 위의 방식은 K 로 나눌 수 없으면 1 씩 빼주는 방식으로 동작합니다.
- 상당히 비효율적입니다.
- 1 씩 빼주지 말고, K 로 나눌 수 있는 크기의 수가 될 때까지 한 번에 빼주면 좀 더 효율적으로 개선할 수 있을 것 같습니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `25 5`.split("\n");

const solution = (input) => {
	let [n, k] = input[0].split(" ").map(Number);

	let ans = 0;
	while (n !== 1) {
		if (n % k === 0) {
			n /= k;
			ans += 1;
		} else {
			let target = parseInt(n / k) * k;
			ans += n - target;
			n = target;
		}
	}

	return ans;
};

console.log(solution(input));
```
