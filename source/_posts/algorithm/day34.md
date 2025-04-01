---
title: Day 34
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 34 Dynamic programming
data: 2025-03-31
---

# Day 34

## [Coin Change II](https://leetcode.com/problems/coin-change-ii/description/)

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length][amount + 1];
        for (int i = 0; i < coins.length; i++) {
            dp[i][0] = 1;
        }

        for (int j = 0; j < amount + 1; j++) {
            if (j % coins[0] == 0)
                dp[0][j] = 1;
        }

        for (int i = 1; i < coins.length; i++) {
            for (int j = 0; j < amount + 1; j++) {
                if (coins[i] > j)
                    dp[i][j] = dp[i - 1][j];
                else
                    dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i]];
            }
        }
        return dp[coins.length - 1][amount];
    }
}
```

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];

        dp[0] = 1;

        for (int i = 0; i < coins.length; i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
}
```

```go
func change(amount int, coins []int) int {
	dp := make([]int, amount+1)
	dp[0] = 1

	for i := 0; i < len(coins); i++ {
		for j := coins[i]; j <= amount; j++ {
			dp[j] += dp[j-coins[i]]
		}
	}

	return dp[amount]
}
```

## [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/description/)

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int j = 1; j <= target; j++) {
            for (int i = 0; i < nums.length; i++) {
                if (j - nums[i] >= 0) dp[j] += dp[j - nums[i]];
            }
        }

        return dp[target];
    }
}
```

```go
func combinationSum4(nums []int, target int) int {
	dp := make([]int, target+1)
	dp[0] = 1
	for j := 1; j <= target; j++ {
		for i := 0; i < len(nums); i++ {
			if j-nums[i] >= 0 {
				dp[j] += dp[j-nums[i]]
			}
		}
	}
	return dp[target]
}

```

## [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

Use full bag solution

```java
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for (int j = 1; j <= n; j++) {
            for (int i = 1; i <= 2; i++) {
                if (j - i >= 0) dp[j] += dp[j - i];
            }
        }
        return dp[n];
    }
}
```

```go
func climbStairs(n int) int {
	dp := make([]int, n+1)
	dp[0] = 1
	for j := 1; j <= n; j++ {
		for i := 1; i <= 2; i++ {
			if j-i >= 0 {
				dp[j] += dp[j-i]
			}
		}
	}
	return dp[n]
}
```

