---
layout: post
title: "BOJ[2531] - 회전 초밥 by JavaScript"
date: 2021-12-11 12:00:00 +0900
categories: BOJ(TwoPointer)
---

# 회전 초밥

## 문제

- [백준 2531번 - 회전 초밥](https://www.acmicpc.net/problem/2531)

## 언어

- JavaScript

## 순서도

1. 완전 탐색 방식으로 모든 경우를 SET 에 넣어서 개수 세기
2. 그 중의 최댓값 구하기

## 문제 풀이 step 1

- 회전 초밥 음식점에는 회전하는 벨트 위에 여러 가지 종류의 초밥이 접시에 담겨 놓여 있습니다.
- 그리고 손님은 이 중에서 자기가 가장 좋아하는 초밥을 골라서 먹습니다.
- 벨트 위에는 같은 종류의 초밥이 둘 이상 있을 수 있습니다.
- 새로 문을 연 회전 초밥 음식점이 불경기로 어려워서, 다음과 같이 두 가지 행사를 통해서 매상을 올리고자 합니다.
  1.  원래 회전 초밥은 손님이 마음대로 초밥을 고르고, 먹은 초밥만큼 식대를 계산하지만, 벨트의 임의의 한 위치부터 k 개의 접시를 연속해서 먹을 경우 할인된 정액 가격으로 제공합니다.
  2.  각 고객에게 초밥의 종류 하나가 쓰인 쿠폰을 발행하고, 1 번 행사에 참가할 경우 이 쿠폰에 적혀진 종류의 초밥 하나를 추가로 무료로 제공합니다. 만약 이 번호에 적혀진 초밥이 현재 벨트 위에 없을 경우, 요리사가 새로 만들어서 손님에게 제공합니다.
- 위 할인 행사에 참여하여 가능한 다양한 종류의 초밥을 먹으려고 합니다.
- 회전 초밥 음식점의 벨트 상태, 메뉴에 있는 초밥의 가짓수, 연속해서 먹는 접시의 개수, 쿠폰 번호가 주어졌을 때, 손님이 먹을 수 있는 초밥 가짓수의 최댓값을 구하는 문제입니다.

## 문제 풀이 step 2

- 완전 탐색 방식을 이용해서 초밥을 먹을 수 있는 모든 경우의 수를 구하고 그 중에서 최댓값을 구하면 됩니다.
- 중복 제거를 위해서 SET 자료구조를 이용해주고, 모든 경우에 쿠폰으로 먹는 초밥도 포함시켜주는 것을 신경써줍니다.
- 모든 경우의 개수를 세어보고 그 중에서 최댓값을 구하면 정답입니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 후기

- 완전 탐색을 하는 방식은 소스코드는 간단하지만, 소요 시간이 너무 오래걸립니다.
- 아래에는 두 포인터로 푸는 방식을 설명해놓았습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const [n, d, k, c] = input[0].split(" ").map(Number);
	const list = input.slice(1).map(Number);

	let max = 1;
	for (let i = 0; i < n; i++) {
		// SET 자료 구조 이용
		const set = new Set();

		let index = i;
		let cnt = k;

		while (cnt-- > 0) {
			if (index === n) index = 0;

			set.add(list[index++]);
		}

		// 쿠폰으로 먹는 초밥 신경써주기
		set.add(c);

		max = Math.max(max, set.size);
	}

	return max;
};

console.log(solution(input));
```

---

## 두 포인터 방식 순서도

1. 두 개의 포인터를 정의하고, 그 범위 안에 있는 초밥의 종류 세기
2. 각 포인터를 한 칸씩 오른쪽으로 이동하며 초밥의 종류의 개수 갱신
3. 쿠폰도 고려하면서 위의 과정 수행
4. 초밥의 종류의 개수의 최댓값 출력

## 두 포인터 방식 문제 풀이 step 1

- 두 개의 포인터를 사용하는 방식으로, 매번 범위 안에 있는 초밥의 종류를 검사할 필요도 없고, SET 자료구조를 사용할 필요도 없습니다.
- 우선 두 개의 포인터를 정의합니다.
- 그리고 포인터의 범위 안에 있는 초밥의 종류를 세어줍니다. 그리고 쿠폰도 고려해줍니다.
- 그리고 각 포인터를 한 칸씩 오른쪽으로 이동하며 초밥의 종류의 개수를 갱신합니다.
  - 왼쪽 포인터가 가리키는 초밥은 제거합니다.
  - 각 포인터를 한 칸씩 오른쪽으로 이동시킵니다.
  - 오른쪽 포인터가 가리키는 초밥은 포함시킵니다.
- 매번 갱신하면서, 초밥의 종류의 최댓값을 구합니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 두 포인터 방식 후기

- 두 포인터를 사용하게 되면, 매번 범위 안에 초밥의 종류의 개수를 검사할 필요가 없기 때문에 알고리즘 성능을 향상시킬 수 있다는 점이 좋은 것 같습니다.

## 두 포인터 방식 소스코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	// 접시의 수, 초밥의 가짓수, 먹는 접시 수, 쿠폰 번호
	const [n, d, k, c] = input[0].split(" ").map(Number);
	const list = input.slice(1).map(Number);

	// [k 번 스시의 총 개수, k 번 스시의 남아있는 수]
	const sushi = Array.from(Array(d + 1), () => Array(2).fill(0));
	for (let i = 0; i < n; i++) {
		sushi[list[i]][0] += 1;
		sushi[list[i]][1] += 1;
	}

	let max = 0;

	// 초기 상태
	let cnt = 0;
	let [left, right] = [0, k - 1];
	for (let i = left; i <= right; i++) {
		const num = list[i];

		// 아직 그 종류가 안팔렸다면,
		if (sushi[num][0] === sushi[num][1]) cnt += 1;
		sushi[num][1] -= 1;
	}

	let coupon = true;
	// 쿠폰의 종류가 안팔렸다면,
	if (sushi[c][0] === sushi[c][1]) {
		coupon = false;

		cnt += 1;
		sushi[c][1] -= 1;
	}

	max = Math.max(max, cnt);

	// 진행 상태
	for (let i = 1; i < n; i++) {
		if (coupon === false) {
			coupon = true;

			// 반납했을 때, 총 개수와 남아있는 수가 같다면,
			if (sushi[c][0] === sushi[c][1] + 1) cnt -= 1;
			sushi[c][1] += 1;
		}

		// 반납했을 때, 총 개수와 남아있는 수가 같다면,
		let num = list[left];
		if (sushi[num][0] === sushi[num][1] + 1) cnt -= 1;
		sushi[num][1] += 1;

		// 포인터 갱신
		[left, right] = [left + 1, right + 1];
		if (right === n) right = 0;

		// 스시의 종류가 안팔렸다면,
		num = list[right];
		if (sushi[num][0] === sushi[num][1]) cnt += 1;
		sushi[num][1] -= 1;

		// 쿠폰을 사용하지 않았고, 쿠폰의 종류가 안팔렸다면,
		if (coupon === true && sushi[c][0] === sushi[c][1]) {
			coupon = false;

			cnt += 1;
			sushi[c][1] -= 1;
		}

		max = Math.max(max, cnt);
	}

	return max;
};

console.log(solution(input));
```
