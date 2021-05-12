---
layout: post
title: "BOJ[15685] - 드래곤 커브 by JavaScript"
date: 2021-05-12 15:30:00 +0900
categories: BOJ(Implementation)
---

# 드래곤 커브

## 문제

- [백준 15685번 - 드래곤 커브](https://www.acmicpc.net/problem/15685)

## 언어

- JavaScript

## 순서도

1. 문제에서 주어진 N 개의 각각의 드래곤 커브를 구성하는 좌표들을 구하기
2. 100 x 100 크기의 격자에 드래곤 커브를 그리기
3. 크기가 1 x 1 이며 네 꼭짓점이 모두 드래곤 커브의 일부인 정사각형의 개수 세기

## 문제 풀이 step 1

- 구현 문제로서, 문제에서 주어진 설명대로 구현하는 문제입니다.
- 가장 고민되는 부분은 드래곤 커브를 구현하는 부분입니다.
- 결론은 K 세대 드래곤 커브는 K - 1 세대 드래곤 커브를 끝 점을 기준으로 90 도 시계 방향 회전 시킨 다음, 그것을 끝점에 붙인 것입니다.
- 이 말은 끝 점 기준으로 이전에 있는 선들을 가지고, 끝 점 기준으로 90 도 시계 방향으로 회전한 방향에서 똑같이 그리면 됩니다.
- 똑같은 말을 그대로 적은 느낌인데, 아래에서 좀더 풀어서 설명하겠습니다.

## 문제 풀이 step 2

- 저는 드래곤 커브를 표현할 때 좌표값을 이용했습니다.
- 문제에서 주어진 사진을 예로 들면,
  ![백준 156585번 드래곤 커브 예제 사진](/public/img/BOJ-Implementation/BOJ-15685-1.JPG)
  - `curve = [[0, 0], [1, 0], [1, -1]];` 과 같이 표현했습니다.
- 그리고 시계 방향에 따라서 좌표를 다음과 같이 구할 수 있습니다.
  ![백준 156585번 시계 방향 사진](/public/img/BOJ-Implementation/BOJ-15685-4.JPG)

## 문제 풀이 step 3

- 우선 0 세대 부터 알아보겠습니다.
  - 0 세대는 문제에서 주어진 시작 점과 시작 방향을 가지고 그리면 됩니다.
  - 추가적으로 무슨 특수한 처리가 필요하진 않습니다.
- 문제는 다음 세대를 구하는 방법입니다. 다음 세대는 위에서 작성한 결론대로 똑같이 구현하면 됩니다.
- 문제에서 주어진 0 세대와 1 세대를 예로 들면,
  - 0 세대 드래곤 커브
    ![백준 156585번 0 세대 드래곤 커브 예제 사진](/public/img/BOJ-Implementation/BOJ-15685-2.JPG)
  - 1 세대 드래곤 커브
    ![백준 156585번 1 세대 드래곤 커브 예제 사진](/public/img/BOJ-Implementation/BOJ-15685-1.JPG)
  - 0 세대 드래곤 커브를 기준으로 1 세대 드래곤 커브를 그리려면
    - 끝 점인 (1, 0) 에서 (0, 0) 으로 가는 방향을 구합니다. 방향은 (-1, 0) 입니다.
    - (-1, 0) 은 9 시 방향을 의미하죠. 여기서 시계 방향으로 90 도 회전하면 12 시 방향이 됩니다.
    - 12 시 방향은 (0, -1) 입니다.
    - 그래서 끝점인 (1, 0) 에서 12 시 방향인 (0, -1) 을 그리면 (1, -1) 이 나옵니다.
  - 이렇게 위에서 작성한 결론의 내용대로 그릴 수 있습니다.
- 이번에는 문제에서 주어진 1 세대와 2 세대를 예로 들면,
  - 1 세대 드래곤 커브
    ![백준 156585번 1 세대 드래곤 커브 예제 사진](/public/img/BOJ-Implementation/BOJ-15685-1.JPG)
  - 2 세대 드래곤 커브
    ![백준 156585번 2 세대 드래곤 커브 예제 사진](/public/img/BOJ-Implementation/BOJ-15685-3.JPG)
  - 1 세대 드래곤 커브를 기준으로 2 세대 드래곤 커브를 그리려면
    - 끝점인 (1, -1) 에서 (1, 0) 으로 가는 방향을 구합니다. 방향은 (0, -1) 입니다.
    - (0, -1) 은 6 시 뱡항을 의미하죠. 여기서 시계 방향으로 90 도 회전하면 9 시 방향이 됩니다.
    - 9 시 방향은 (-1, 0) 입니다.
    - 그래서 끝점인 (1, -1) 에서 9 시 방향인 (-1, 0) 을 그리면 (0, -1) 이 나옵니다.
    - 그리고
    - 이번에는 끝점이 2 개가 됩니다.
    - 다음 선분을 그리기 위한 끝점 (0, -1) 과 과거의 방향을 파악하기 위한 끝점 (1, 0) 입니다.
    - 우선 과거의 방향을 파악하기 위해서 (1, 0) 에서 (0, 0) 으로 가는 방향을 구합니다. 방향은 (-1, 0) 입니다.
    - (-1, 0) 은 9 시 방향을 의미하죠. 여기서 시계 방향으로 90도 회전하면 12 시 방향이 됩니다.
    - 12 시 방향은 (0, -1) 입니다.
    - 그래서 다음 선분을 그리기 위한 끝점 (0, -1) 에서 12 시 방향인 (0, -1) 을 그리면 (0, -2) 가 나옵니다.

## 문제 풀이 step 4

- 위와 같은 과정으로 모든 드래곤 커브를 그립니다.
- 그리고 100 x 100 격자에 드래곤 커브를 구성하는 좌표들을 전부 찍어줍니다.
- 그리고 네 꼭짓점이 모두 드래곤 커브의 일부인 1 x 1 크기의 정사각형의 개수를 모두 세어주고, 출력하면 정답이 됩니다.
- 추가적인 설명은 주석에 작성하겠습니다.

## 후기

- 역시 구현 문제는 펜으로 그려보면서 특징을 파악하는 게 많은 도움이 되는 것 같습니다.

## 소스 코드

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

// 드래곤 커브의 세대를 늘리는 함수
const addGeneration = (curve) => {
	// 시계 방향 구현 : 값이 5 개인 이유는 % 처리를 대신하기 위해서
	const clockwiseX = [0, -1, 0, 1, 0];
	const clockwiseY = [1, 0, -1, 0, 1];

	// 과거의 방향을 파악하기 위한 끝점
	let [beforeX, beforeY] = curve[curve.length - 1];

	// 끝 점을 기준으로 이전의 점들을 한 개씩 검사
	for (let i = curve.length - 2; i >= 0; i--) {
		// 다음 선분을 그리기 위한 끝점
		let [lateX, lateY] = curve[curve.length - 1];

		let [nowX, nowY] = curve[i];
		// 과거의 방향 구하기
		let [cx, cy] = [nowX - beforeX, nowY - beforeY];

		let nextIdx = 0;
		// 구한 과거의 방향을 시계 방향으로 90 도 회전
		for (let j = 0; j < 5; j++) {
			if (cx === clockwiseX[j] && cy === clockwiseY[j]) {
				nextIdx = j + 1;
				break;
			}
		}

		// 구한 방향으로 새로운 정점 생성
		let [nextX, nextY] = [
			lateX + clockwiseX[nextIdx],
			lateY + clockwiseY[nextIdx],
		];

		// 새로운 정점 추가
		curve.push([nextX, nextY]);

		[beforeX, beforeY] = [nowX, nowY];
	}
};

// 100 x 100 크기의 격자에 드래곤 커브를 그리는 함수
const paint = (n, curves, grid) => {
	for (let i = 0; i < n; i++) {
		const curve = curves[i];

		for (let j = 0; j < curve.length; j++) {
			const [x, y] = curve[j];
			grid[x][y] = true;
		}
	}
};

// 100 x 100 크기의 격자에서 네 꼭짓점이 모두 드래곤 커브의 일부인 1 x 1 크기의 정사각형의 개수를 세는 함수
const count = (n, grid) => {
	let cnt = 0;
	for (let i = 0; i < 100; i++) {
		for (let j = 0; j < 100; j++) {
			if (grid[i][j] && grid[i + 1][j] && grid[i][j + 1] && grid[i + 1][j + 1])
				cnt += 1;
		}
	}

	return cnt;
};

const solution = (input) => {
	const n = parseInt(input[0]);
	const datas = input.slice(1).map((v) => v.split(" ").map(Number));

	const curves = [];
	// 문제에서 주어진 4 개의 방향 구현
	const cx = [1, 0, -1, 0];
	const cy = [0, -1, 0, 1];
	for (let i = 0; i < n; i++) {
		curves[i] = [];
		let [x, y, d, g] = datas[i];

		// 시작점을 이용한 0 세대 드래곤 커브 구현
		curves[i].push([x, y]);
		curves[i].push([x + cx[d], y + cy[d]]);

		// 1 씩 세대를 늘려가며 드래곤 커브 구현
		for (let j = 1; j <= g; j++) {
			addGeneration(curves[i]);
		}
	}

	// 100 x 100 크기의 격자를 만든 후, 모든 드래곤 커브를 그림
	const grid = Array.from({length: 101}, () => Array(101).fill(false));
	paint(n, curves, grid);

	// 100 x 100 크기의 격자에서 네 꼭짓점이 모두 드래곤 커브의 일부인 1 x 1 크기의 정사각형의 개수를 return
	return count(n, grid);
};

console.log(solution(input));
```
