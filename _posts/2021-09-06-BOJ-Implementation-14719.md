---
layout: post
title: "BOJ[14719] - 빗물 by JavaScript"
date: 2021-09-06 12:00:00 +0900
categories: BOJ(Implementation)
---

# 빗물

## 문제

- [백준 14719번 - 빗물](https://www.acmicpc.net/problem/14719)

## 언어

- JavaScript

## 순서도

1. 높이가 h 일때부터 1 일때까지
2. 가장 왼쪽에서 오른쪽으로 이동하며 기둥과 기둥 사이의 공간에 물 채우기

## 문제 풀이 step 1

- 본 문제는 시뮬레이션 형식의 문제로, 문제에서 주어진 조건대로 로직을 짜는 문제입니다.
- 2 차원 세계에 블록이 쌓여 있습니다. 충분히 많은 비가 올 때, 고이는 빗물의 총량을 구하는 문제입니다.
- 물이 고이기 위해서는 웅덩이가 있어야 하고, 웅덩이가 있기 위해서는 왼쪽과 오른쪽에 블록이 쌓여있어야 합니다.
- 우선 가장 높은 위치부터 한 줄씩 검사를 진행합니다.
  - 한 줄 안에 왼쪽과 오른쪽에 블록이 있다면, 그 사이의 공간에는 물이 고입니다.
  - 따라서, 한 줄을 가장 왼쪽부터 가장 오른쪽으로 한 칸씩 검사해가며, 왼쪽과 오른쪽에 블록이 있는 공간들을 모두 찾아서 물을 넣어주고, 물의 양을 세어주면 됩니다.
- 최종적으로 고인 물의 양을 출력하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 보통 w, h 순서로 입력을 주는 데, 이번에는 h, w 순서로 입력을 줘서 무지성으로 풀다가 시행착오를 겪었습니다.
- 제가 푼 방법은 각각의 높이마다 한 행씩 검사를 하는 방식인데, 이와 다르게 각각의 칸마다 한 열씩 검사를 하는 방식도 있습니다.
  - 각각의 칸마다 한 열씩 검사를 하는 방식은 현 위치에서 왼쪽에서 가장 높은 기둥, 오른쪽에서 가장 높은 기둥을 찾고, 그 중에서 낮은 기둥만큼 물을 넣는 방식입니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const [h, w] = input[0].split(" ").map(Number);
	const blocks = input[1].split(" ").map(Number);

	let water = 0;
	// 가장 높은 곳부터 한 행씩 검사
	for (let height = h; height > 0; height--) {
		let before = -1;
		// 왼쪽 끝에서 오른쪽 끝으로 한 칸씩 이동해가며, 기둥과 기둥 사이인 곳에 빗물 넣기
		for (let i = 0; i < w; i++) {
			if (blocks[i] >= height) {
				if (before !== -1) {
					water += i - before - 1;
				}

				before = i;
			}
		}
	}

	return water;
};

console.log(solution(input));
```
