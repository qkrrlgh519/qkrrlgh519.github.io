---
layout: post
title: "BOJ[20327] - 배열 돌리기 6 by JavaScript"
date: 2021-07-15 12:00:00 +0900
categories: BOJ(Implementation)
---

# 배열 돌리기 6

## 문제

- [백준 20327번 - 배열 돌리기 6](https://www.acmicpc.net/problem/20327)

## 언어

- JavaScript

## 순서도

1. 1 ~ 8 번 연산에 대한 함수 구현
2. 입력으로 주어진 배열에 R개의 연산을 순서대로 수행
3. 결과 배열을 문자열 형태로 출력

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 1번 연산은 각 부분 배열에 대해서 상하 반전시키면 됩니다.
- 2번 연산은 각 부분 배열에 대해서 좌우 반전시키면 됩니다.
- 5번 연산은 부분 배열을 한 칸으로 생각하고 상하 반전시키면 됩니다.
- 6번 연산은 부분 배열을 한 칸으로 생각하고 좌우 반전시키면 됩니다.
- 조금 어려웠던 부분은 3번 연산과 7번 연산입니다.
- 4번 연산과 8번 연산은 3번 연산과 7번 연산에서 대입하는 배열과 대입되는 배열을 서로 바꾸기만 하면 됩니다.
- 따라서 3번 연산과 7번 연산에 대해서 중점적으로 알아보겠습니다.

## 문제 풀이 step 2

- 3번 연산은 각 부분 배열을 오른쪽으로 90도 회전시켜야 합니다.
- 차근히 배열의 인덱스의 변화를 살펴보겠습니다.
- before 배열을 오른쪽으로 90도 회전시키면 after 배열처럼 index 가 변하게 됩니다.
- before 배열과 after 배열의 각 요소의 index 의 차이를 나타낸 것이 diff 배열입니다.
  - 편하게 보기 위해서 배열의 요소를 문자열로 표현하게 되었습니다.

```jsx
// 부분 배열의 크기가 4 인 경우
const before = [
	"[0, 0], [0, 1], [0, 2], [0, 3]",
	"[1, 0], [1, 1], [1, 2], [1, 3]",
	"[2, 0], [2, 1], [2, 2], [2, 3]",
	"[3, 0], [3, 1], [3, 2], [3, 3]",
];
const after = [
	"[3, 0], [2, 0], [1, 0], [0, 0]",
	"[3, 1], [2, 1], [1, 1], [0, 1]",
	"[3, 2], [2, 2], [1, 2], [0, 2]",
	"[3, 3], [2, 3], [1, 3], [0, 3]",
];
const diff = [
	"[3, 0], [2, -1], [1, -2], [0, -3]",
	"[2, 1], [1, 0], [0, -1], [-1, -2]",
	"[1, 2], [0, 1], [-1, 0], [-2, -1]",
	"[0, 3], [-1, 2], [-2, 1], [-3, 0]",
];
const element = ["[x, y]"];
```

- diff 배열을 보면서 우리는 규칙을 찾을 수 있습니다.
- 우선 x 좌표가 어떻게 변하는지 보겠습니다.
  - x 좌표의 초기값은 "부분 배열의 크기 - 1" 입니다.
  - 그리고 x 좌표는 오른쪽으로 갈수록 1 씩 감소합니다. 마찬가지로 아래로 내려갈수록 1 씩 감소합니다.
- 그 다음 y 좌표가 어떻게 변하는지 보겠습니다.
  - y 좌표의 초기값은 "0" 입니다.
  - 그리고 y 좌표는 오른쪽으로 갈수록 1 씩 감소합니다. 반면에 아래로 내려갈수록 1 씩 증가합니다.
- 이렇게 알아본 x 좌표와 y 좌표의 변화를 코드로 표현하면 아래와 같습니다.

```jsx
// step 은 부분 배열의 크기를 의미
for (let x = i; x < i + step; x++) {
	let xDiff = step - 1 - (x - i); // x 좌표의 변화량을 의미
	let yDiff = 0 + (x - i); // y 좌표의 변화량을 의미
	for (let y = j; y < j + step; y++) {
		res[x][y] = arr[x + xDiff--][y + yDiff--];
	}
}
```

- 3 번 연산은 이렇게 구현할 수 있고, 4 번 연산은 두 번째 for 문 안에 있는 res 와 arr 배열의 index 를 서로 봐꿔주기만 하면 됩니다.
- `res[x + xDiff--][y + yDiff--] = arr[x][y];`

## 문제 풀이 step 3

- 7번 연산은 부분 배열을 한 칸으로 생각하고 오른쪽으로 90도 회전시켜야 합니다.
- 차근히 배열의 인덱스의 변화를 살펴보겠습니다.
- before 배열의 각 부분 배열을 한 칸으로 생각하고, 오른쪽으로 90도 회전시키면 after 배열처럼 index 가 변하게 됩니다.
- before 배열과 after 배열의 각 요소의 index 의 차이를 나타낸 것이 diff 배열입니다.
  - 편하게 보기 위해서 배열의 요소를 문자열로 표현하게 되었습니다.

```jsx
// 부분 배열의 크기가 2 인 경우
const before = [
	"[[0, 0], [0, 1]], [[0, 2], [0, 3]], [[0, 4], [0, 5]], [[0, 6], [0, 7]]",
	"[[1, 0], [1, 1]], [[1, 2], [1, 3]], [[1, 4], [1, 5]], [[1, 6], [1, 7]]",
	"[[1, 0], [1, 1]], [[1, 2], [1, 3]], [[1, 4], [1, 5]], [[1, 6], [1, 7]]",
	"[[1, 0], [1, 1]], [[1, 2], [1, 3]], [[1, 4], [1, 5]], [[1, 6], [1, 7]]",
	"5 번째 행...",
	"6 번째 행...",
	"7 번째 행...",
	"8 번째 행...",
];
const after = [
	"[[6, 0], [6, 1]], [[4, 0], [4, 1]], [[2, 0], [2, 1]], [[0, 0], [0, 1]]",
	"[[7, 0], [7, 1]], [[5, 0], [5, 1]], [[3, 0], [3, 1]], [[1, 0], [1, 1]]",
	"[[6, 2], [6, 3]], [[4, 2], [4, 3]], [[2, 2], [2, 3]], [[0, 2], [0, 3]]",
	"[[7, 2], [7, 3]], [[5, 2], [5, 3]], [[3, 2], [3, 3]], [[1, 2], [1, 3]]",
	"5 번째 행...",
	"6 번째 행...",
	"7 번째 행...",
	"8 번째 행...",
];
const diff = [
	"[[6, 0], [6, 0]], [[4, -2], [4, -2]], [[2, -4], [2, -4]], [[0, -6], [0, -6]]",
	"[[6, 0], [6, 0]], [[4, -2], [4, -2]], [[2, -4], [2, -4]], [[0, -6], [0, -6]]",
	"[[4, 2], [4, 2]], [[2, 0], [2, 0]], [[0, -2], [0, -2]], [[-2, -4], [-2, -4]]",
	"[[4, 2], [4, 2]], [[2, 0], [2, 0]], [[0, -2], [0, -2]], [[-2, -4], [-2, -4]]",
	"5 번째 행...",
	"6 번째 행...",
	"7 번째 행...",
	"8 번째 행...",
];
const element = ["[x, y]"];
```

- 이번에도 diff 배열을 보면서 우리는 규칙을 찾을 수 있습니다.
- 우선 x 좌표가 어떻게 변하는지 보겠습니다.
  - 부분 배열을 한 칸으로 보기 때문에 부분 배열 내에서는 변화가 없습니다.
  - x 좌표의 초기값은 "전체 배열의 크기 - 부분 배열의 크기" 입니다.
  - 그리고 x 좌표는 오른쪽으로 갈수록 "부분 배열의 크기" 만큼씩 감소합니다. 마찬가지로 아래로 내려갈수록 "부분 배열의 크기"만큼씩 감소합니다.
- 그 다음 y 좌표가 어떻게 변하는지 보겠습니다.
  - 마찬가지로 부분 배열을 한 칸으로 보기 때문에 부분 배열 내에서는 변화가 없습니다.
  - y 좌표의 초기값은 "0" 입니다.
  - 그리고 y 좌표는 오른쪽으로 갈수록 "부분 배열의 크기" 만큼씩 감소합니다. 반면에 아래로 내려갈수록 "부분 배열의 크기"만큼씩 증가합니다.
- 이렇게 알아본 x 좌표와 y 좌표의 변화를 코드로 표현하면 아래와 같습니다.

```jsx
let xBase = len - step;
let yBase = 0;
for (let i = 0; i < len; i += step) {
	let xDiff = xBase;
	let yDiff = yBase;
	// 부분 배열을 한 칸으로 생각하고 배열을 오른쪽으로 90도 회전
	for (let j = 0; j < len; j += step) {
		for (let x = i; x < i + step; x++) {
			for (let y = j; y < j + step; y++) {
				res[x][y] = arr[x + xDiff][y + yDiff];
			}
		}
		xDiff -= step;
		yDiff -= step;
	}
	xBase -= step;
	yBase += step;
}
```

- 7 번 연산은 이렇게 구현할 수 있고, 8 번 연산은 두 번째 for 문 안에 있는 res 와 arr 배열의 index 를 서로 바꿔주기만 하면 됩니다.
- `res[x + Diff][y + diff] = arr[x][y]`;

## 문제풀이 step 4

- 주어지는 배열에 위에서 구현한 각 연산 함수를 적용시켜서 연산을 수행하고, 그 결과값을 출력하면 정답이 됩니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 본 문제를 푸는데 3시간 이상 사용했습니다. 너무 아쉽고 다음에 풀 때는 좀 더 빠르게 풀 수 있기를 기원합니다.
- 역시 이런 문제는 **index 를 다 나열**해 보면서 **어떻게 변하는지 파악**하는 것이 중요한 것 같습니다.
- 마치 아이큐 문제처럼 어떤 수의 나열을 보고 그 변화를 파악하는 문제와 유사한 것 같습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const operators = {
	1: (len, arr, l) => {
		if (l === 0) return arr;

		const res = Array.from({length: len}, () => []);
		const step = Math.pow(2, l);

		for (let i = 0; i < len; i += step) {
			for (let j = 0; j < len; j += step) {
				// 각 부분 배열을 상하 반전
				for (let y = j; y < j + step; y++) {
					for (let x = i; x < i + step; x++) {
						res[x][y] = arr[i + step - 1 - (x - i)][y];
					}
				}
			}
		}

		return res;
	},
	2: (len, arr, l) => {
		if (l === 0) return arr;

		const res = Array.from({length: len}, () => []);
		const step = Math.pow(2, l);

		for (let i = 0; i < len; i += step) {
			for (let j = 0; j < len; j += step) {
				// 각 부분 배열을 좌우 반전
				for (let x = i; x < i + step; x++) {
					for (let y = j; y < j + step; y++) {
						res[x][y] = arr[x][j + step - 1 - (y - j)];
					}
				}
			}
		}

		return res;
	},
	3: (len, arr, l) => {
		if (l === 0) return arr;

		const res = Array.from({length: len}, () => []);
		const step = Math.pow(2, l);

		for (let i = 0; i < len; i += step) {
			for (let j = 0; j < len; j += step) {
				// 각 부분 배열을 오른쪽으로 90도 회전
				for (let x = i; x < i + step; x++) {
					let xDiff = step - 1 - (x - i);
					let yDiff = 0 + (x - i);
					for (let y = j; y < j + step; y++) {
						res[x][y] = arr[x + xDiff--][y + yDiff--];
					}
				}
			}
		}

		return res;
	},
	4: (len, arr, l) => {
		if (l === 0) return arr;

		const res = Array.from({length: len}, () => []);
		const step = Math.pow(2, l);

		for (let i = 0; i < len; i += step) {
			for (let j = 0; j < len; j += step) {
				// 각 부분 배열을 왼쪽으로 90도 회전
				// 3번 연산에서 res 의 index 와 arr 의 index 를 서로 바꿔주기
				for (let x = i; x < i + step; x++) {
					let xDiff = step - 1 - (x - i);
					let yDiff = 0 + (x - i);
					for (let y = j; y < j + step; y++) {
						res[x + xDiff--][y + yDiff--] = arr[x][y];
					}
				}
			}
		}

		return res;
	},
	5: (len, arr, l) => {
		const res = Array.from({length: len}, () => []);
		const step = Math.pow(2, l);

		let xDiff = len - step;
		for (let i = 0; i < len; i += step) {
			for (let j = 0; j < len; j += step) {
				// 부분 배열을 한 칸으로 생각하고 배열을 상하 반전
				for (let x = i; x < i + step; x++) {
					for (let y = j; y < j + step; y++) {
						res[xDiff + (x - i)][y] = arr[x][y];
					}
				}
			}

			xDiff -= step;
		}

		return res;
	},
	6: (len, arr, l) => {
		const res = Array.from({length: len}, () => []);
		const step = Math.pow(2, l);

		let yDiff = len - step;
		for (let j = 0; j < len; j += step) {
			for (let i = 0; i < len; i += step) {
				// 부분 배열을 한 칸으로 생각하고 배열을 좌우 반전
				for (let x = i; x < i + step; x++) {
					for (let y = j; y < j + step; y++) {
						res[x][yDiff + (y - j)] = arr[x][y];
					}
				}
			}

			yDiff -= step;
		}

		return res;
	},
	7: (len, arr, l) => {
		const res = Array.from({length: len}, () => []);
		const step = Math.pow(2, l);

		let xBase = len - step;
		let yBase = 0;
		for (let i = 0; i < len; i += step) {
			let xDiff = xBase;
			let yDiff = yBase;
			// 부분 배열을 한 칸으로 생각하고 배열을 오른쪽으로 90도 회전
			for (let j = 0; j < len; j += step) {
				for (let x = i; x < i + step; x++) {
					for (let y = j; y < j + step; y++) {
						res[x][y] = arr[x + xDiff][y + yDiff];
					}
				}
				xDiff -= step;
				yDiff -= step;
			}
			xBase -= step;
			yBase += step;
		}

		return res;
	},
	8: (len, arr, l) => {
		const res = Array.from({length: len}, () => []);
		const step = Math.pow(2, l);

		let xBase = len - step;
		let yBase = 0;
		for (let i = 0; i < len; i += step) {
			let xDiff = xBase;
			let yDiff = yBase;
			// 부분 배열을 한 칸으로 생각하고 배열을 왼쪽으로 90도 회전
			// 7번 연산에서 res 의 index 와 arr 의 index 를 서로 바꿔주기
			for (let j = 0; j < len; j += step) {
				for (let x = i; x < i + step; x++) {
					for (let y = j; y < j + step; y++) {
						res[x + xDiff][y + yDiff] = arr[x][y];
					}
				}
				xDiff -= step;
				yDiff -= step;
			}
			xBase -= step;
			yBase += step;
		}

		return res;
	},
};

const solution = (input) => {
	const [n, r] = input[0].split(" ").map(Number);

	const len = Math.pow(2, n);
	let arr = input.slice(1, len + 1).map((v) => v.split(" ").map(Number));

	for (let i = len + 1; i < len + 1 + r; i++) {
		const [k, l] = input[i].split(" ").map(Number);
		arr = operators[k](len, arr, l);
	}

	return arr.map((v) => v.join(" ")).join("\n");
};

console.log(solution(input));
```
