---
layout: post
title: "BOJ[1924] - 2007년 by JavaScript"
date: 2021-04-26 13:00:00 +0900
categories: BOJ(Implementation)
---

# 2007년

## 문제

- [백준 1924번 - 2007년](https://www.acmicpc.net/problem/1924)

## 언어

- JavaScript

## 순서도

1. 주어진 x 월 y 일을 총 일수로 변환
2. 총 일수를 7 로 나머지 연산
3. 나머지를 통해서 무슨 요일인지 판별

## 문제 풀이 step 1

- 문제에서 원하는 것은 2007 년 x 월 y 일의 요일입니다.
- 문제에서 2007 년의 각 월의 일수 정보와 2007 년 1 월 1 일의 요일 정보를 제공해주었습니다.

## 문제 풀이 step 2

- x 월 y 일이 1 월 1 일 부터 몇 일이 지났는지 파악을 한다면 요일 정보를 알 수 있을 것입니다.
- 그래서 1 월 부터 x 월 y 일 까지의 총 일수를 파악합니다.
- 그리고 요일은 7 일 단위로 반복이 되기 때문에 7 로 나눠줍니다.
- 그 나머지 값을 이용해서 요일 정보를 출력해주면 정답이 됩니다.

## 문제 풀이 step 3

- 이런 유형에서 제 개인적으로 중요하다고 생각하는 것은 문제에서 제공한 정보를 배열화 해서 쉽게 접근할 수 있도록 만드는 것입니다.
  - **일 수 정보** : `const months = [null, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];`
  - **요일** : `const days = ["SUN", "MON", "TUE", "WED", "THU", "FRI", "SAT"];`
- 이렇게 해놓으면 로직을 진행하기 편해지고, 코드도 간결해집니다.

## 후기

- 여태까지 본 코딩테스트와 최근에 본 N 사의 코딩테스트에서 구현 유형이 기본 베이스인 느낌을 많이 받아서 구현 유형의 문제 풀이를 시작합니다.

## 소스 코드 1

```jsx
const input = require("fs").readFileSync("/dev/stdin").toString().split("\n");

const solution = (input) => {
	const [x, y] = input[0].split(" ").map(Number);

	const months = [null, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
	const days = ["SUN", "MON", "TUE", "WED", "THU", "FRI", "SAT"];

	let cnt = y;
	for (let i = 1; i < x; i++) {
		cnt += months[i];
	}
	cnt %= 7;

	return days[cnt];
};

console.log(solution(input));
```
