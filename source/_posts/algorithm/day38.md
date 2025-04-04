---
title: Day 38
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 38 Dynamic programming
data: 2025-04-04
---

# Day 38

## [Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/)

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int[][] dp = new int[prices.length][2 * k + 1];

        for (int j = 1; j < 2 * k; j += 2) {
            dp[0][j] = -prices[0];
        }

        for (int i = 1; i < prices.length; i ++) {
            for (int j = 0; j < 2 * k - 1; j += 2) {
                dp[i][j + 1] = Math.max(dp[i - 1][j + 1], dp[i - 1][j] - prices[i]);
                dp[i][j + 2] = Math.max(dp[i - 1][j + 2], dp[i - 1][j + 1] + prices[i]);
            }
        }
        return dp[prices.length - 1][2 * k];  
    }
}
```

```go
func maxProfit(k int, prices []int) int {
	dp := make([][]int, len(prices))
	for i := 0; i < len(prices); i++ {
		dp[i] = make([]int, 2*k+1)
	}

	for j := 1; j < 2*k; j += 2 {
		dp[0][j] = -prices[0]
	}

	for i := 1; i < len(prices); i++ {
		for j := 0; j < 2*k-1; j += 2 {
			dp[i][j+1] = max(dp[i-1][j+1], dp[i-1][j]-prices[i])
			dp[i][j+2] = max(dp[i-1][j+2], dp[i-1][j+1]+prices[i])
		}
	}
	return dp[len(prices)-1][2*k]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

## [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int[][] dp = new int[prices.length][4];
        dp[0][0] = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            // buy state
            dp[i][0] = Math.max(dp[i - 1][0], Math.max(dp[i - 1][3] - prices[i], dp[i - 1][1] - prices[i]));
            // sell state
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][3]);
            // sell today!
            dp[i][2] = dp[i - 1][0] + prices[i];
            // cooldown
            dp[i][3] = dp[i - 1][2];
        }

        return Math.max(dp[prices.length - 1][3], Math.max(dp[prices.length - 1][1], dp[prices.length - 1][2]));
    }
}
```

```go
func maxProfit(prices []int) int {
	dp := make([][]int, len(prices))
	for i := 0; i < len(prices); i++ {
		dp[i] = make([]int, 4)
	}

	dp[0][0] = -prices[0]
	for i := 1; i < len(prices); i++ {
		// buy state
		dp[i][0] = max(dp[i-1][0], max(dp[i-1][1]-prices[i], dp[i-1][3]-prices[i]))
		// sell state
		dp[i][1] = max(dp[i-1][1], dp[i-1][3])
		// sell today!
		dp[i][2] = dp[i-1][0] + prices[i]
		// cooldown
		dp[i][3] = dp[i-1][2]
	}
	return slices.Max(dp[len(prices)-1])
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

```

## [Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int[][] dp = new int[prices.length][2];
        dp[0][0] = -prices[0];

        for (int i = 1; i < prices.length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee);
        }
        return dp[prices.length - 1][1];
    }
}
```

```go
func maxProfit(prices []int, fee int) int {
	dp := make([][]int, len(prices))
	for i := 0; i < len(prices); i++ {
		dp[i] = make([]int, 2)
	}
	dp[0][0] = -prices[0]
	for i := 1; i < len(prices); i++ {
		dp[i][0] = max(dp[i-1][0], dp[i-1][1]-prices[i])
		dp[i][1] = max(dp[i-1][1], dp[i-1][0]+prices[i]-fee)
	}
	return dp[len(prices)-1][1]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

