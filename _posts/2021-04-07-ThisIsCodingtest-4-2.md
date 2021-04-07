---
layout: post
title: "이것이 코딩테스트다 - [Implementation] 시각 by JavaScript"
date: 2021-04-07 15:30:00 +0900
categories: ThisIsCodingtest(NDB)
---

# 시각

## 출처

- [[책] 이것이 코딩테스트다](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[GitHub] 이것이 코딩테스트다](https://github.com/ndb796/python-for-coding-test)

## 언어

- JavaScript

## 문제 풀이 step 1

- 문제에서 주어진 대로 구현하는 구현 유형의 문제입니다.
- 간단하게 반복문을 이용해서 "0 시 0 분 0 초" 에서 "1 초"씩 늘려가면서 "N 시 59 분 59 초"가 될 때까지 "3" 이 포함된 횟수를 구하면 정답이 됩니다.

## 소스 코드

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");
// const input = `5`.split("\n");

checkThree = (num) => {
	for (let i = 0; i < num.length; i++) {
		if (num[i] === "3") return true;
	}
	return false;
};

const solution = (input) => {
	const n = parseInt(input[0]);

	let ans = 0;
	for (let h = 0; h < n + 1; h++) {
		if (checkThree(h.toString())) {
			ans += 3600;
			continue;
		}

		for (let m = 0; m < 60; m++) {
			if (checkThree(m.toString())) {
				ans += 60;
				continue;
			}

			for (let s = 0; s < 60; s++) {
				if (checkThree(s.toString())) ans += 1;
			}
		}
	}

	return ans;
};

console.log(solution(input));
```
