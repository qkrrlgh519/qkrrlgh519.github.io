---
layout: post
title: "BOJ[1244] - 스위치 켜고 끄기 by JavaScript"
date: 2021-07-30 13:00:00 +0900
categories: BOJ(Implementation)
---

# 스위치 켜고 끄기

## 문제

- [백준 1244번 - 스위치 켜고 끄기](https://www.acmicpc.net/problem/1244)

## 언어

- JavaScript

## 순서도

1. 입력에 따라서 스위치를 켜고 끄고, 그 결과를 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 문제에서 주어진 스위치를 켜고 끄는 방식에 따라서 스위치를 켜고 끄고, 그 결과 상태를 출력하는 문제입니다.
- 스위치에 관한 정보는
  - 스위치는 1 부터 연속적으로 번호가 붙어 있습니다.
  - 1 은 켜져 있음을 0 은 꺼져 있음을 나타냅니다.
  - 남학생은 자신이 받은 수의 배수 번호의 스위치의 상태를 바꿉니다.
  - 여학생은 자신이 받은 수의 번호의 스위치를 기준으로 좌우 대칭이면서 가장 많은 스위치를 포함하는 구간의 상태를 바꿉니다.

## 문제 풀이 step 2

- 우선, 주어진 입력에 따라서 각 스위치의 번호를 담은 배열을 생성합니다.
  - 현재는 0 ~ N - 1 까지의 번호로 구성되어 있습니다.
  - 문제에서 입력받은 번호를 그대로 이용하기 위해서 본 배열의 맨 앞에 임의의 값을 아무거나 넣어줍니다. 저는 - 1 을 넣었습니다.
  - 이렇게 하면 배열에서 스위치는 1 ~ N 까지의 번호로 접근이 가능하게 됩니다.
  - 대신 출력하기 전에 맨 앞의 값을 제거해줘야 합니다.
- 그리고 학생의 성별에 따라서 스위치를 켜고 끕니다.
  - 남학생의 경우는 주어진 번호의 배수 번호의 스위치를 켜고 꺼주면 됩니다.
  - 여학생의 경우는 주어진 번호를 기준으로 좌측과 우측으로 한 칸씩 확장해가면서 좌우 대칭이 되는 범위까지 찾고, 그 범위에 있는 스위치들을 켜고 꺼주면 됩니다.
- 위 과정을 통해서 변경된 스위치의 상태를 출력해주면 정답입니다.
  - 이 때, 조심해야 할 것은 한 줄에 20 개씩 출력해야 합니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 배열의 index 를 잘 조작해야 하는 문제였습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const n = Number(input[0]);
	const switches = input[1].split(" ").map(Number);
	// 문제에서 주어진 번호를 그대로 이용하기 위해서 배열의 맨 앞에 임의의 값 추가
	switches.unshift(-1);
	let k = Number(input[2]);

	let index = 3;
	while (k-- > 0) {
		let [gender, num] = input[index++].split(" ").map(Number);

		// 남학생의 경우
		if (gender === 1) {
			// 주어진 번호의 배수 번호의 스위치들의 상태를 변경
			for (let i = num; i <= n; i += num) {
				if (switches[i] === 0) switches[i] = 1;
				else switches[i] = 0;
			}
		}

		// 여학생의 경우
		if (gender === 2) {
			// 우선, 주어진 번호의 스위치의 상태를 변경
			if (switches[num] === 0) switches[num] = 1;
			else switches[num] = 0;

			let left = num;
			let right = num;
			// 주어진 번호를 기준으로 좌와 우에서 대칭인 범위의 스위치의 상태를 변경
			while (1 < left-- && right++ < n) {
				if (switches[left] !== switches[right]) break;

				if (switches[left] === 0) [switches[left], switches[right]] = [1, 1];
				else [switches[left], switches[right]] = [0, 0];
			}
		}
	}

	// 아까 배열의 맨 앞에 넣은 임의의 값 제거
	switches.shift();

	// 한 줄에 20 개씩 출력하기 위한 작업
	let res = "";
	for (let i = 0; i < n; i++) {
		if ((i + 1) % 20 === 0) res += switches[i] + "\n";
		else res += switches[i] + " ";
	}

	return res;
};

console.log(solution(input));
```
