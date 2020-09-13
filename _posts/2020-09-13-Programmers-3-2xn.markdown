---
layout: post
title:  "Programmers - 2 x n 타일링"
date:   2020-09-13 21:00:00 +0900
categories: Programmers(3)
---

# 2 x n 타일링

## 환경

- Programmers
- Java

## 문제 풀이 step 1

- 다이나믹 프로그래밍으로 접근했습니다.
- 다이나믹 프로그래밍으로 풀 때는 n-k만큼의 상황은 이미 완성되어 있고 k를 넣어서 n을 완성시키는 상황을 생각해야 합니다.
- 그리고 그 생각을 가지고 점화식을 만들면 됩니다.

## 문제 풀이 step 2

- n을 만들기 위해서 "n-1 만큼 완성"이 되어있다면 세로 타일 1개를 채우면 될 것입니다.
- n을 만들기 위해서 "n-2 만큼 완성"이 되어있다면 가로 타일 2개를 채우면 될 것입니다. 세로 타일을 2개 채우는 경우의 수는 위의 "n-1 만큼 완성"이 되어있는 경우에 포함 됩니다.
- 점화식 : dp[n] = dp[n-1] + dp[n-2]

## 소스 코드

```java
class Solution {
    public int solution(int n) {
        final int MOD = 1000000007;
        int answer = 0;
        
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i=2; i<n+1; i++){
            dp[i] = (dp[i-1] + dp[i-2]) % MOD;
        }
        
        answer = dp[n];
        return answer;
    }
}
```