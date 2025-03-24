---
title: Day 32
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 32 Dynamic programming
data: 2025-03-24
---

# Day 32

## 01 bag

Ming is a scientist who needs to attend an important international scientific conference to present his latest research. He needs to bring some research materials with him, but he has limited space in his suitcase. These research materials include experimental equipment, literature, experimental samples, etc., each of which occupies a different space and has a different value. 

Ming's luggage space is N. Ask Ming how he should choose to carry the most valuable research materials. Each research material can only be chosen once, and there are only two choices: choose or not choose, no cutting.

Input：

```
The first row contains two positive integers, the first integer, M, representing the type of research material and the second positive integer, N, representing Ming's luggage space.
The second row contains M positive integers representing the space occupied by each type of research material. 
The third row contains M positive integers representing the value of each research material.
```

Output:

```
Output an integer representing the maximum value of research material that Ming can carry.
```

Example:

```
6 1
2 2 3 1 5 2
2 3 1 5 4 3
```

Result:

```
5
```

Code:

2-dimension dp array

```java
class Solution {
    public int bag(int[] values, int[] weights, int n, int bagWeight) {
        int[][] dp = new int[n][bagWeight + 1];
        // Init the dp array with only put the first object.
        for (int j = weights[0]; j <= bagWeight; j++) {
            dp[0][j] = values[0];
        }
        for (int i = 1; i < n; i++) {
            for (int j = 0; j <= bagWeight; j++) {
                if (j < weights[i]) {
                    // if current weight can't tale i object, value is same as dp[i - 1][j].
                    dp[i][j] = dp[i - 1][j];
                } else {
                    // put: dp[i - 1][j - weights[i]] + values[i]
                    // not put: dp[i - 1][j];
                    dp[i][j] = Math.max(dp[i - 1][j - weights[i]] + values[i], dp[i - 1][j]);
                }
            }
        }
        return dp[n - 1][bagWeight];
    }
}
// @lc code=end

```

```go
func bag(values, weights []int, n, bagWeight int) int {
	dp := make([][]int, n)
	for i := range dp {
		dp[i] = make([]int, bagWeight+1)
	}
	for j := weights[0]; j <= bagWeight; j++ {
		dp[0][j] = values[0]
	}
	for i := 1; i < n; i++ {
		for j := 0; j <= bagWeight; j++ {
			if j < weights[i] {
				dp[i][j] = dp[i-1][j]
			} else {
				dp[i][j] = max(dp[i-1][j], dp[i-1][j-weights[i]]+values[i])
			}
		}
	}
	return dp[n-1][bagWeight]
}

```

1-dimension dp array

```java
class Solution {
    public int bag(int[] values, int[] weights, int n, int bagWeight) {
        int[] dp = new int[bagWeight + 1];
        for (int i = 0; i < n; i++) {
            for (int j = bagWeight; j >= weights[i]; j--) {
                dp[j] = Math.max(dp[j], dp[j - weights[i]] + values[i]);
            }
        }
        return dp[bagWeight];
    }
}
// @lc code=end
```

```go
func bag(values, weights []int, n, bagWeight int) int {
	dp := make([]int, bagWeight+1)
	for i := 0; i < n; i++ {
		for j := bagWeight; j >= weights[i]; j-- {
			dp[j] = max(dp[j], dp[j-weights[i]]+values[i])
		}
	}
	return dp[bagWeight]
}

```

## [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/description/)

```java
class Solution {
    public boolean canPartition(int[] nums) {
        if (nums.length == 0 || nums == null) {
            return false;
        }
        int n = nums.length;
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        int space = sum / 2;
        int[] dp = new int[space + 1];
        for (int i = 0; i < n; i++) {
            for (int j = space; j >= nums[i]; j--) {
                dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
            }

            if (dp[space] == space) {
                return true;
            }
        }
        return false;
    }
}
```

```go
func canPartition(nums []int) bool {
	sum := 0
	for _, num := range nums {
		sum += num
	}
	if sum%2 == 1 {
		return false
	}

	space := sum / 2
	dp := make([]int, space+1)
	for _, num := range nums {
		for j := space; j >= num; j-- {
			dp[j] = max(dp[j], dp[j-num]+num)
		}

		if dp[space] == space {
			return true
		}
	}
	return false
}
```

