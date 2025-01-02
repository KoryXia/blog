---
title: Day 29
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 29 Dynamic programming
data: 2025-01-02
---

# Day 29

Five steps to solve dynamic programming problems:

1. Determine a `dp` array and the meaning of the array index.
2. Determine the recurrence formula.
3. Init the `dp` array.
4. Determine the recursion order.
5. Example derivation `dp` array.

## [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/description/)

```java
class Solution {
    public int fib(int n) {
        if (n <= 1) {
            return n;
        }

        int a = 0, b = 1;

        for (int i = 2; i <= n; i++) {
            int sum = a + b;
            a = b;
            b = sum;
        }
        return b;
    }
}
```

```go
func fib(n int) int {
	if n <= 1 {
		return n
	}
	a, b := 0, 1
	for i := 2; i <= n; i++ {
		sum := a + b
		a = b
		b = sum
	}
	return b
}
```

## [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 2) {
            return n;
        } 
        int[] dp = new int[n+1];
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}
```

```go

func climbStairs(n int) int {
	if n <= 2 {
		return n
	}

	dp := make([]int, n+1)
	dp[1] = 1
	dp[2] = 2
	for i := 3; i <= n; i++ {
		dp[i] = dp[i-1] + dp[i-2]
	}
	return dp[n]
}
```

## [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/description/)

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] dp = new int[cost.length + 1];
        dp[0] = 0;
        dp[1] = 0;

        for (int i = 2; i <= cost.length; i++) {
            dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        return dp[cost.length];
    }
}
```

```go
func minCostClimbingStairs(cost []int) int {
	dp := make([]int, len(cost)+1)
	dp[0], dp[1] = 0, 0

	for i := 2; i <= len(cost); i++ {
		dp[i] = min(dp[i-1]+cost[i-1], dp[i-2]+cost[i-2])
	}
	return dp[len(cost)]
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

