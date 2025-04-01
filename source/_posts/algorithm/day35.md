---
title: Day 35
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 35 Dynamic programming
data: 2025-04-01
---

# Day 35

## [Coin Change](https://leetcode.com/problems/coin-change/description/)

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;

        for (int i = 0; i < coins.length; i++) {
            for (int j = coins[i]; j <= amount; j++) {
                if (dp[j - coins[i]] != Integer.MAX_VALUE) {
                    dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
                }
            }
        }
        return dp[amount] == Integer.MAX_VALUE ? -1 : dp[amount];
    }
}
```

```go
func coinChange(coins []int, amount int) int {
	dp := make([]int, amount+1)
	for i := 1; i <= amount; i++ {
		dp[i] = math.MaxInt32
	}

	for i := 0; i < len(coins); i++ {
		for j := coins[i]; j <= amount; j++ {
			if dp[j-coins[i]] != math.MaxInt32 {
				dp[j] = min(dp[j], dp[j-coins[i]]+1)
			}
		}
	}
	if dp[amount] == math.MaxInt32 {
		return -1
	}

	return dp[amount]
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

## [Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

```java
import java.util.Arrays;

class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
    
        Arrays.fill(dp, Integer.MAX_VALUE);

        dp[0] = 0;

        for (int i = 1; i * i <= n; i++) {
            for (int j = i * i; j <= n; j++) {
                dp[j] = Math.min(dp[j], dp[j - i * i] + 1);
            }
        }

        return dp[n];
    }
}
```

```go
func numSquares(n int) int {
	dp := make([]int, n+1)

	dp[0] = 0

	for i := 1; i <= n; i++ {
		dp[i] = math.MaxInt32
	}

	for i := 0; i*i <= n; i++ {
		for j := i * i; j <= n; j++ {
			dp[j] = min(dp[j], dp[j-i*i]+1)
		}
	}
	return dp[n]
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

## [Word Break](https://leetcode.com/problems/word-break/description/)

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length() + 1];

        dp[0] = true;

        for (int i = 1; i <= s.length(); i++) {
            for (String word : wordDict) {
                int len = word.length();
                if (i >= len && dp[i - len] && s.substring(i - len, i).equals(word)) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
```

```go
func wordBreak(s string, wordDict []string) bool {
	dp := make([]bool, len(s)+1)

	dp[0] = true

	for i := 1; i <= len(s); i++ {
		for _, v := range wordDict {
			length := len(v)
			if i >= length && dp[i-length] && s[i-length:i] == v {
				dp[i] = true
				break
			}
		}
	}
	return dp[len(s)]
}
```

