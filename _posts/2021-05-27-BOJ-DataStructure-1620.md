---
layout: post
title: "BOJ[1620] - 나는야 포켓몬 마스터 이다솜 by JavaScript"
date: 2021-05-27 12:00:00 +0900
categories: BOJ(DataStructure)
---

# 나는야 포켓몬 마스터 이다솜

## 문제

- [백준 1620번 - 나는야 포켓몬 마스터 이다솜](https://www.acmicpc.net/problem/1620)

## 언어

- JavaScript

## 순서도

1. 주어지는 N 마리의 포켓몬에 대해서 Map 을 이용해서 `포켓몬 이름 (key) : 포켓몬 번호 (value)` 쌍으로 데이터를 저장합니다.
2. 주어지는 N 마리의 포켓몬에 대해서 Array 를 이용해서 `포켓몬 번호 (index) : 포켓몬 이름 (value)` 형태로 데이터를 저장합니다.
3. 1 번 과 2 번에서 생성한 Map 과 Array 를 이용해서 M 개의 줄에 대한 답을 말합니다.

## 문제 풀이 step 1

- 이다솜은 포켓몬 마스터가 되기 위해서 포켓몬 도감을 완성시키려고 합니다.
- 포켓몬 도감은 포켓몬의 이름을 입력하면 포켓몬의 번호를 출력하거나, 포켓몬의 번호를 입력하면 포켓몬의 이름을 출력합니다.
- 포켓몬 도감에 포켓몬의 정보가 key 와 value 형태로 저장된다는 것을 알 수 있습니다.
- 그렇다면 가장 잘 어울리는 자료 구조는 Map 임을 알 수 있습니다.
  - 두 개의 Map 을 이용해서 하나는 `포켓몬 이름 (key) : 포켓몬 번호 (value)` 쌍으로 데이터를 저장하고, 다른 하나는 `포켓몬 번호 (key) : 포켓몬 이름 (value)` 쌍으로 데이터를 저장해서 도감을 만듭니다.
  - 저는 한 개의 Map 과 한 개의 Array 를 이용해서 도감을 만들었습니다. (Array 의 index 를 일종의 key 로 볼 수 있기 때문입니다.)
- 이렇게 도감을 만들고, M 개의 문제에 대한 답을 출력하면 정답이 됩니다.
- 추가 설명은 주석에 작성하겠습니다.

## 소스 코드 1

```jsx
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

const solution = (input) => {
	const [n, m] = input[0].split(" ").map(Number);

	// Map 과 Array 를 준비
	const nameMap = new Map();
	const numberArr = [];

	// Map 과 Array 를 이용해서 도감 생성
	for (let i = 1; i < n + 1; i++) {
		nameMap.set(input[i], i);
		numberArr[i] = input[i];
	}

	// M 개의 문제에 대한 답 생성
	let res = "";
	for (let i = n + 1; i < n + 1 + m; i++) {
		if (nameMap.get(input[i])) res += nameMap.get(input[i]) + "\n";
		else res += numberArr[Number(input[i])] + "\n";
	}

	return res;
};

console.log(solution(input));
```
