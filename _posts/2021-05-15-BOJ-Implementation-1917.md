---
layout: post
title: "BOJ[1917] - 정육면체 전개도 by JavaScript"
date: 2021-05-15 14:00:00 +0900
categories: BOJ(Implementation)
---

# 정육면체 전개도

## 문제

- [백준 1917번 - 정육면체 전개도](https://www.acmicpc.net/problem/1917)

## 언어

- JavaScript

## 순서도

1. 11 가지의 전개도 타입 정의
2. 각 전개도 타입마다 뒤집는 경우 (2) 와 회전하는 경우 (4) 를 모두 확인해서 주어진 입력에 일치하는 전개도가 있는지 확인
3. 출력 양식에 맞게 문자열 출력

## 문제 풀이 step 1

- 구현 문제이지만 완전 탐색 기법으로 풀 수 있는 문제였습니다.
- 정육면체의 전개도는 뒤집는 경우와 회전하는 경우를 하나로 생각하면 다음 사진과 같이 총 11 가지 종류가 있습니다.
  ![백준 1917번 11 가지 종류의 정육면체 전개도 사진](/public/img/BOJ-Implementation/BOJ-1917-1.JPG)
- 위의 사진과 같이 11 가지 종류의 정육면체 전개도 타입을 정의합니다.
- 그리고 주어진 입력에서 11 가지 종류의 정육면체 전개도와 일치하는 것이 있는지 검사하면 됩니다.
  - 정방향 x 4 번 (시계 방향 90 도 회전)
  - 뒤집은 방향 x 4 번 (시계 방향 90 도 회전)
- 위와 같이 8 번씩 11 가지 모두 검사해줍니다.
- 일치하는 것이 있으면 'yes' 를 없으면 'no' 를 출력해주면 됩니다.

## 후기

- 다시 풀어볼 문제입니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 11 가지 종류의 정육면체 전개도 타입
const types = [
	[
		[1, 0, 0, 0],
		[1, 1, 1, 1],
		[1, 0, 0, 0],
	],
	[
		[0, 1, 0, 0],
		[1, 1, 1, 1],
		[1, 0, 0, 0],
	],
	[
		[0, 0, 1, 0],
		[1, 1, 1, 1],
		[1, 0, 0, 0],
	],
	[
		[0, 0, 0, 1],
		[1, 1, 1, 1],
		[1, 0, 0, 0],
	],
	[
		[0, 1, 0, 0],
		[1, 1, 1, 1],
		[0, 1, 0, 0],
	],
	[
		[0, 0, 1, 0],
		[1, 1, 1, 1],
		[0, 1, 0, 0],
	],
	[
		[0, 0, 1, 1, 1],
		[1, 1, 1, 0, 0],
	],
	[
		[0, 0, 1, 1],
		[0, 1, 1, 0],
		[1, 1, 0, 0],
	],
	[
		[0, 0, 1, 1],
		[1, 1, 1, 0],
		[1, 0, 0, 0],
	],
	[
		[1, 1, 0, 0],
		[0, 1, 1, 1],
		[0, 1, 0, 0],
	],
	[
		[0, 1, 0, 0],
		[1, 1, 1, 0],
		[0, 0, 1, 1],
	],
];

// 전개도를 시계방향으로 90 도 회전하는 함수
const rotate = (arr) => {
	const res = Array.from({length: arr[0].length}, () => []);

	for (let j = 0; j < arr[0].length; j++) {
		for (let i = arr.length - 1; i >= 0; i--) {
			res[j][arr.length - 1 - i] = arr[i][j];
		}
	}

	return res;
};

// 전개도를 뒤집는 함수 (좌 <-> 우)
const flip = (arr) => {
	const res = [];
	for (let i = 0; i < arr.length; i++) {
		res[i] = arr[i].reverse();
	}

	return res;
};

// 주어진 입력에서 전개도 타입과 일치하는 것이 있는지 확인하는 함수
const sameCheck = (arr, type) => {
	const [row, col] = [type[0].length, type.length];

	for (let i = 0; i <= 6 - col; i++) {
		for (let j = 0; j <= 6 - row; j++) {
			let isSame = true;

			// 일치하는 전개도가 있는지 확인
			for (let k = 0; k < col; k++) {
				for (let l = 0; l < row; l++) {
					if (arr[i + k][j + l] === type[k][l]) continue;

					isSame = false;
					break;
				}
			}

			if (isSame === true) return true;
		}
	}

	return false;
};

// 11 가지의 전개도 마다 2 번 뒤집고 4 번 회전하며 검사하는 함수
const check = (arr) => {
	for (let i = 0; i < 11; i++) {
		let type = types[i];

		for (let j = 0; j < 2; j++) {
			for (let k = 0; k < 4; k++) {
				if (sameCheck(arr, type) === true) return true;
				// 회전하기
				type = rotate(type);
			}
			// 뒤집기
			type = flip(type);
		}
	}

	return false;
};

const solution = (input) => {
	let res = "";

	for (let i = 0; i < 3; i++) {
		const arr = input
			.slice(i * 6, i * 6 + 6)
			.map((v) => v.split(" ").map(Number));

		if (check(arr) === true) res += "yes\n";
		else res += "no\n";
	}

	return res;
};

console.log(solution(input));
```
