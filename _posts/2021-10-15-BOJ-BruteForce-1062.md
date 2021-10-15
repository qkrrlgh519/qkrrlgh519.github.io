---
layout: post
title: "BOJ[1062] - 가르침 by JavaScript"
date: 2021-10-15 12:00:00 +0900
categories: BOJ(BruteForce)
---

# 가르침

## 문제

- [백준 1062번 - 가르침](https://www.acmicpc.net/problem/1062)

## 언어

- JavaScript

## 순서도

1. 가르치는 단어의 개수가 5 이상인지 검사하여 분기처리
2. 주어지는 단어들에서 앞에서 "anta" 를, 뒤에서 "tica" 를 제거한 후에 Set 에 넣기
3. 'a', 'n', 't', 'i',' c' 를 제외한 알파벳들 중에서 k - 5 개를 가르치는 모든 경우의 수 만들기
4. 그 경우의 수 중에서 단어를 최대로 읽을 수 있는 개수 세기

## 문제 풀이 step 1

- 본 문제는 완전 탐색 문제로, 모든 경우의 수를 만들어보고 그 경우 중에서 최적의 값을 찾는 문제입니다.
- 김지민 선생님은 학생들이 되도록이면 많은 단어를 읽을 수 있도록 하려고 합니다.
- 김지민 선생님은 학생들에게 K 개의 글자를 가르치려고 합니다.
- 학생들은 배움을 받은 후, 배운 K 개의 글자로만 이루어진 단어만 읽을 수 있습니다.
- 김지민 선생님은 어떤 K 개의 글자를 가르쳐야 학생들이 최대한 많은 단어를 읽을 수 있을까요??
- 남극 언어의 모든 단어는 "anta" 로 시작되고, "tica" 로 끝나고, 남극언어에 단어는 N 개밖에 없다고 합니다.

## 문제 풀이 step 2

- 우선, 남극언어의 모든 단어는 "anta" 로 시작되고, "tica" 로 끝나기 때문에 기본적으로 'a', 'n', 't', 'i', 'c' 이 5 개의 글자를 가르쳐야 합니다.
- 따라서, K 가 5 보다 작다면 어떤 단어도 읽을 수 없기 때문에 조건 처리를 해줍니다.
- 그리고 위의 5 개의 글자를 제외하고, K - 5 개를 가르치는 모든 경우를 만들어봅니다.
- 그리고 각 경우마다 주어지는 단어들 중에서 읽을 수 있는 단어의 개수를 세서 그 최댓값을 출력하면 정답입니다.

## 문제 풀이 step 3

- 전체적인 로직은 위와 같고, 중간 중간 소요시간을 줄이기 위한 로직이 필요합니다.
- 하나는 주어지는 단어를 매번 모두 검사하지 않게 하는 것입니다.
  - 주어지는 단어에서 앞과 뒤에서 'anta' 와 'tica' 를 제외하고, 남은 단어를 Set 에 넣어서 글자 중복을 제거합니다.
  - 추가로 단순 index 검사를 위해서 Set 에 넣을 때, 문자 자체를 넣지 말고 char code 값을 넣어줍니다.
- 두 번째는 'a', 'b', ..., 'z' 까지 총 26 개의 글자를 가르치지 않습니다.
  - 글자를 가르치는 모든 경우를 만들 때 26 개의 글자를 가르치지 않게 'a', 'n', 't', 'i', 'c' 이 5 개의 글자를 제외합니다.
  - 이렇게 하면 총 21 개의 글자만 가르치게 됩니다.
- 이 정도의 로직을 추가해주고, 나머지는 위에서 설명한대로 로직을 구현하면 됩니다.
- 추가 설명은 주석으로 작성하겠습니다.

## 소스 코드

```javascript
const input = require("fs")
	.readFileSync("/dev/stdin")
	.toString()
	.trim()
	.split("\n");

let max = 0;

const rec = (k, words, learned, alphabets, index, depth) => {
	// k 개의 단어를 모두 가르쳤다면
	if (depth === k) {
		// 주어진 단어들 중에서 읽을 수 있는 단어의 개수 세기
		let cnt = 0;
		for (let i = 0; i < words.length; i++) {
			let isOk = true;
			for (let letter of words[i]) {
				if (learned[letter] === false) {
					isOk = false;
					break;
				}
			}

			if (isOk) cnt += 1;
		}

		// 최댓값 갱신
		max = Math.max(max, cnt);
		return;
	}

	if (index >= alphabets.length) return;

	learned[alphabets[index]] = true;
	rec(k, words, learned, alphabets, index + 1, depth + 1);

	learned[alphabets[index]] = false;
	rec(k, words, learned, alphabets, index + 1, depth);
};

const solution = (input) => {
	const [n, k] = input[0].split(" ").map(Number);
	const inputWords = input.slice(1);

	// k 가 5 개 이하라면, 어떤 단어도 읽을 수 없음
	if (k < 5) return 0;

	let res = 0;
	const words = [];
	for (let i = 0; i < n; i++) {
		// 단어에서 앞의 "anta" 와 뒤의 "tica" 를 제외시키기
		const word = inputWords[i].slice(4, inputWords[i].length - 4);

		// 제외 후 빈 문자열이 나오면, 이는 읽을 수 있는 단어이므로 개수 증가
		if (word === "") res += 1;
		// 제외 후 남은 단어에서 글자의 중복 없애기
		else
			words.push(
				new Set(word.split("").map((v) => v.charCodeAt(0) - "a".charCodeAt(0)))
			);
	}

	const learned = Array(26).fill(false);
	let alphabets = Array(26)
		.fill(0)
		.map((v, i) => v + i);

	// 기본 글자 5 개 제외시키기
	const base = ["a", "n", "t", "i", "c"];
	for (let i = 0; i < 5; i++) {
		learned[base[i].charCodeAt(0) - "a".charCodeAt(0)] = true;
		alphabets[base[i].charCodeAt(0) - "a".charCodeAt(0)] = -1;
	}
	alphabets = alphabets.filter((v) => v !== -1);

	// 재귀함수를 통해서 글자를 가르치는 모든 경우 만들어보기
	rec(k - 5, words, learned, alphabets, 0, 0);

	return res + max;
};

console.log(solution(input));
```
