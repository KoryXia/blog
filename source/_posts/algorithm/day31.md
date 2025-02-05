---
title: Day 31
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 31 Dynamic programming
data: 2025-01-03
---

# Day 31

## [Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/description/)

The number of left subtrees is `j - 1` and the number of right subtrees is `i - j`.

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
}
```

```go
func numTrees(n int) int {
	dp := make([]int, n+1)
	dp[0] = 1
	for i := 1; i <= n; i++ {
		for j := 1; j <= i; j++ {
			dp[i] += dp[j-1] * dp[i-j]
		}
	}
	return dp[n]
}
```
