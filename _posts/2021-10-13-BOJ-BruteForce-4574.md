---
layout: post
title: "BOJ[4574] - 스도미노쿠 by JavaScript"
date: 2021-10-13 12:00:00 +0900
categories: BOJ(BruteForce)
---

# 스도미노쿠

## 문제

- [백준 4574번 - 스도미노쿠](https://www.acmicpc.net/problem/4574)

## 언어

- JavaScript

## 순서도

1. 모든 테스트케이스에 대해서
2. 주어진 퍼즐 정보와 9 개의 숫자 정보를 스도쿠에 넣고, 이와 동시에 행 정보, 열 정보, 격자 정보, 도미노 정보 갱신하기
3. 스도쿠에서 빈 칸 정보 모두 수집하기
4. 모든 빈 칸에 대해서 넣을 수 있는 모든 퍼즐을 넣어보고, 스도쿠가 해결되는 경우 문자열 형태로 기록하기
5. 모든 테스트케이스의 문자열 출력하기

## 문제 풀이 step 1

- 본 문제는 완전 탐색 문제로, 경우의 수가 너무 많기 때문에 백트래킹 로직도 적용해줘야 하는 문제입니다.
- 스도미노쿠란 스도쿠와 도미노를 혼합한 게임입니다.
- 스도미노쿠는 스도쿠 규칙을 따릅니다.
  - 9 x 9 크기의 그리드를 1 부터 9 까지 숫자를 이용해서 채워야 합니다.
  - 각 행에는 1 부터 9 까지 숫자가 하나씩 있어야 합니다.
  - 각 열에는 1 부터 9 까지 숫자가 하나씩 있어야 합니다.
  - 3 x 3 크기의 정사각형에는 1 부터 9 까지 숫자가 하나씩 있어야 합니다.
- 스도미노쿠의 그리드에는 1 부터 9 까지의 숫자가 쓰여있고, 나머지 72 칸은 도미노 타일 36 개로 채워야 합니다.
- 도미노 타일은 1 부터 9 까지 서로 다른 숫자의 가능한 쌍이 모두 포함되어 있습니다. ([1, 2], [1, 3], [1, 4], ..., [7, 9], [8, 9])
  - [1, 2] 와 [2, 1] 은 같은 타일이기 때문에, 따로 구분하지 않습니다.
  - 도미노 타일은 회전시킬 수 있고, 3 x 3 크기의 정사각형의 경계에 걸쳐서 놓여질 수도 있습니다.
- 스도미노쿠의 퍼즐의 초기 상태가 주어졌을 때, 퍼즐을 푸는 문제입니다.

## 문제 풀이 step 2

- 본 문제는 [백준 2580번 - 스도쿠 풀이](<https://qkrrlgh519.github.io/boj(bruteforce)/2021/08/03/BOJ-BruteForce-2580.html>) 에 추가로 도미노 타일을 고려해야 하는 문제입니다.
- 우선 기본 로직은 백준 2550번 스도쿠와 동일하고, 추가로 도미노 타일을 검사하는 로직을 추가하면 되겠습니다.
- 기본적으로 스도미노쿠는 1 ~ 9 까지의 숫자와 n 개의 타일이 쓰여져있습니다.
  - 따라서 9 x 9 크기의 스도쿠를 생성하고, 1 ~ 9 까지의 숫자와 n 개의 타일을 입력합니다.
  - 이와 동시에 행, 열, 격자, 도미노 정보를 갱신합니다.
  - 예를 들어, 0 행 3 열에 7 이 들어간다면,
    - row[0][7] = true
    - col[3][7] = true
    - grid[3 x parseInt(0 / 3) + parseInt(3 / 3)][7] = true
  - 예를 들어, [1, 2] 도미노를 사용했다면,
    - domino[1][2] = true
- 그리고 위의 정보를 모두 입력한 스도쿠에서 빈 칸 정보를 모두 수집합니다.
- 그리고 재귀함수를 수행합니다.
  - 재귀함수는 모든 빈 칸에 대해서 도미노 타일을 적용시켜보고, 스도쿠가 풀리는 경우 문자열 형태로 저장합니다.
  - 우선, 각 빈 칸에 대해서 36 개의 도미노를 적용시켜봅니다.
  - 도미노는 정방향과 역방향이 있고, 가로와 세로로 놓는 경우가 있습니다.
    - 예를 들어, [1, 2] 의 경우 정방향은 [1, 2], 역방향은 [2, 1]
    - 예를 들어, [x, y] 의 경우 가로는 [x, y], [x, y + 1], 세로는 [x, y], [x + 1, y]
  - 따라서 각 빈 칸에 36 개의 도미노를 정방향과 역방향 그리고 가로와 세로로 놓아봅니다.
    - 매 경우마다 도미노 체크, 행 체크, 열 체크, 격자 체크를 진행합니다.
      - 이미 사용한 도미노인지
      - 놓으려는 도미노의 숫자가 이미 행에 있는지
      - 놓으려는 도미노의 숫자가 이미 열에 있는지
      - 놓으려는 도미노의 숫자가 이미 격자에 있는지
    - 도미노 체크, 행 체크, 열 체크, 격자 체크를 통과했다면 도미노를 놓고, 재귀를 통해서 다음 칸으로 넘어갑니다.
    - 이렇게 재귀를 진행해가며 빈 칸을 모두 채웁니다.
  - 빈 칸이 모두 채워지고, 스도쿠가 풀리게 되면, 이를 문자열의 형태로 저장합니다.
- 모든 테스트케이스에 대해서 재귀함수를 수행하고, 푼 스도쿠를 모두 문자열의 형태로 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 꼭 다시 풀어볼 문제입니다.
- 이 문제를 푸는 데, 1.5 일 정도 걸렸습니다.
- 로직은 정확하다고 생각해서, 오타 수정을 계속 반복했지만 풀리지 않았습니다.
- 결국 다른 분의 풀이를 참고 했는데, 재귀의 특징을 정확하게 이해하지 못해서 발생한 문제였습니다.
- 소스 코드에는 만약 스도쿠에 숫자가 이미 채워져 있다면, depth 만 1 증가시키고, 다음 재귀로 넘어가는 로직이 있습니다.
  - 이 때, 아래의 코드처럼 분기 처리를 해야 하는데,
  ```javascript
  if (sdoku[x1][y1] !== 0) {
  	// depth 를 1 증가시키고, 다음 재귀로 넘어가는 로직
  } else {
  	// 36 개의 도미노를 적용하는 로직
  }
  ```
  - 아래의 코드처럼 분기 처리를 하지 않아서, 문제가 풀리지 않았습니다.
  ```javascript
  if (sdoku[x1][y1] !== 0) {
  	// depth 를 1 증가시키고, 다음 재귀로 넘어가는 로직
  }
  // 36 개의 도미노를 적용하는 로직
  ```
  - 이는 재귀를 통해서 들어갔다가 다시 나오는 순간, 아래의 로직이 수행되어서 이미 채워져 있는 칸에 36 개의 도미노를 적용하는 오류가 발생해서입니다.
- 재귀 알고리즘을 좀 더 공부하고, 익숙해져야겠다는 생각을 하게 되었습니다.
- 그리고 도미노를 놓는 경우를 상, 하, 좌, 우 이렇게 4 개의 경우를 생각했는데, 이러면 중복 적용이 발생해서 불필요한 검사가 수행되었습니다.
  - 그럴 필요 없이 도미노를 놓는 경우를 하, 우 이렇게 2 개의 경우만 생각해도, 모든 경우를 커버할 수 있었습니다.
  - 또한 알고리즘 수행 시간도 줄일 수 있었습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

let ans = null;

// 스도쿠 칸을 넘어가는지 검사하는 함수
const isPossibleRoute = (x, y) => 0 <= x && x < 9 && 0 <= y && y < 9;

// 재귀 함수
const solveSdoku = (blanks, sdoku, row, col, grid, domino, depth) => {
	if (ans !== null) return;

	// 빈 칸을 모두 채웠으면 문자열 기록
	if (depth === blanks.length) {
		ans = sdoku.map((v) => v.join("")).join("\n");
		return;
	}

	const [x1, y1] = blanks[depth];

	if (sdoku[x1][y1] !== 0) {
		// 스도쿠 칸이 이미 채워져 있다면, 재귀 함수 수행
		solveSdoku(blanks, sdoku, row, col, grid, domino, depth + 1);
	} else {
		// 채워져 있지 않다면 36 개의 도미노 적용
		for (let i = 1; i < 9; i++) {
			for (let j = i + 1; j < 10; j++) {
				// 도미노 체크
				if (domino[i][j] === true) continue;

				// 정방향과 역방향 도미노
				const dom = [
					[i, j],
					[j, i],
				];

				// 정방향과 역방향 도미노에 대해
				for (let k = 0; k < 2; k++) {
					const [n1, n2] = dom[k];

					if (row[x1][n1] === true) continue;
					if (col[y1][n1] === true) continue;
					if (grid[3 * parseInt(x1 / 3) + parseInt(y1 / 3)][n1] === true)
						continue;

					sdoku[x1][y1] = n1;
					row[x1][n1] = true;
					col[y1][n1] = true;
					grid[3 * parseInt(x1 / 3) + parseInt(y1 / 3)][n1] = true;

					// 가로와 세로
					const dir = [
						[x1, y1 + 1],
						[x1 + 1, y1],
					];

					// 가로와 세로에 대해
					for (let l = 0; l < 2; l++) {
						const [x2, y2] = dir[l];

						if (!isPossibleRoute(x2, y2)) continue;
						if (sdoku[x2][y2] !== 0) continue;

						if (row[x2][n2] === true) continue;
						if (col[y2][n2] === true) continue;
						if (grid[3 * parseInt(x2 / 3) + parseInt(y2 / 3)][n2] === true)
							continue;

						// 행, 열, 격자, 도미노 체크를 모두 통과했다면, 스도쿠에 기록 후 재귀
						sdoku[x2][y2] = n2;
						row[x2][n2] = true;
						col[y2][n2] = true;
						grid[3 * parseInt(x2 / 3) + parseInt(y2 / 3)][n2] = true;

						domino[i][j] = true;

						solveSdoku(blanks, sdoku, row, col, grid, domino, depth + 1);

						// 재귀함수에서 나올 때, 기록했던 정보 다시 초기화
						domino[i][j] = false;

						sdoku[x2][y2] = 0;
						row[x2][n2] = false;
						col[y2][n2] = false;
						grid[3 * parseInt(x2 / 3) + parseInt(y2 / 3)][n2] = false;
					}

					sdoku[x1][y1] = 0;
					row[x1][n1] = false;
					col[y1][n1] = false;
					grid[3 * parseInt(x1 / 3) + parseInt(y1 / 3)][n1] = false;
				}
			}
		}
	}
};

const solution = (input) => {
	let res = "";

	let testCase = 1;
	let index = 0;

	// 각 테스트케이스에 대해서
	while (true) {
		let n = Number(input[index++]);
		if (n === 0) break;

		// 스도쿠, 행, 열, 격자, 도미노 체크를 위한 배열 생성
		const sdoku = Array.from({length: 9}, () => Array(9).fill(0));
		const row = Array.from({length: 9}, () => Array(10).fill(false));
		const col = Array.from({length: 9}, () => Array(10).fill(false));
		const grid = Array.from({length: 9}, () => Array(10).fill(false));
		const domino = Array.from({length: 10}, () => Array(10).fill(false));

		// 주어진 n 개의 퍼즐 입력
		while (n-- > 0) {
			const str = input[index++].split(" ");
			const [n1, n2] = [str[0], str[2]].map(Number);
			const [[x1, y1], [x2, y2]] = [str[1], str[3]]
				.map((v) => v.split(""))
				.map((v) => [v[0].charCodeAt(0) - "A".charCodeAt(0), Number(v[1]) - 1]);

			const arr = [
				[x1, y1, n1],
				[x2, y2, n2],
			];

			if (n1 <= n2) domino[n1][n2] = true;
			else domino[n2][n1] = true;

			for (let i = 0; i < 2; i++) {
				const [x, y, n] = arr[i];

				sdoku[x][y] = n;
				row[x][n] = true;
				col[y][n] = true;
				grid[3 * parseInt(x / 3) + parseInt(y / 3)][n] = true;
			}
		}

		// 주어지는 9 개의 수 입력
		const base = input[index++].split(" ");
		for (let i = 0; i < 9; i++) {
			let [x, y] = base[i].split("");
			[x, y] = [x.charCodeAt(0) - "A".charCodeAt(0), Number(y) - 1];

			sdoku[x][y] = i + 1;
			row[x][i + 1] = true;
			col[y][i + 1] = true;
			grid[3 * parseInt(x / 3) + parseInt(y / 3)][i + 1] = true;
		}

		// 스도쿠에서 빈 칸 모두 찾기
		const blanks = [];
		for (let i = 0; i < 9; i++) {
			for (let j = 0; j < 9; j++) {
				if (sdoku[i][j] === 0) blanks.push([i, j]);
			}
		}

		// 재귀 함수 수행
		ans = null;
		solveSdoku(blanks, sdoku, row, col, grid, domino, 0);

		res += `Puzzle ${testCase++}\n${ans}\n`;
	}

	return res;
};

console.log(solution(input));
```
