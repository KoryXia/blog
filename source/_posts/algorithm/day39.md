---
title: Day 39
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 39 Dynamic programming
data: 2025-04-07
---

# Day 39

## [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);
        int res = 1;
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```

```go
func lengthOfLIS(nums []int) int {
	dp := make([]int, len(nums))
	res := 1
	for i := 0; i < len(nums); i++ {
		dp[i] = 1
	}

	for i := 1; i < len(nums); i++ {
		for j := 0; j < i; j++ {
			if nums[i] > nums[j] {
				dp[i] = max(dp[i], dp[j]+1)
			}
		}
		res = max(res, dp[i])
	}
	return res
}
func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}

```

## [Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);
        int res = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > nums[i - 1]) {
                dp[i] = Math.max(dp[i], dp[i - 1] + 1);
                res = Math.max(res, dp[i]);
            }
        }
        return res;
    }
}
```

```go
func findLengthOfLCIS(nums []int) int {
	dp, res := make([]int, len(nums)), 1
	for i := range dp {
		dp[i] = 1
	}
	for i := 1; i < len(nums); i++ {
		if nums[i] > nums[i-1] {
			dp[i] = max(dp[i], dp[i-1]+1)
		}
		res = max(res, dp[i])
	}
	return res
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

## [Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/description/)

```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int[][] dp = new int[nums1.length + 1][nums2.length + 1];
        int res = 0;
        for (int i = 1; i < nums1.length + 1; i++) {
            for (int j = 1; j < nums2.length + 1; j++) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                    res = Math.max(res, dp[i][j]);
                }
            }
        }
        return res;
    }
}
```

```go
func findLength(A []int, B []int) int {
	m, n := len(A), len(B)
	res := 0
	dp := make([][]int, m+1)
	for i := 0; i <= m; i++ {
		dp[i] = make([]int, n+1)
	}

	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if A[i-1] == B[j-1] {
				dp[i][j] = dp[i-1][j-1] + 1
			}
			if dp[i][j] > res {
				res = dp[i][j]
			}
		}
	}
	return res
}
```

