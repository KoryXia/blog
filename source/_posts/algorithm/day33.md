---
title: Day 33
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 33 Dynamic programming
data: 2025-03-25
---

# Day 33

## [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/description/)

```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int num : stones) {
            sum += num;
        }
        int space = sum / 2;
        int[] dp = new int[space + 1];

        for (int i = 0; i < stones.length; i++) {
            for (int j = space; j >= stones[i]; j--) {
                dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
            }
        }
        return sum - 2 * dp[space];
    }
}
```

```go
func lastStoneWeightII(stones []int) int {
	sum := 0
	for _, stone := range stones {
		sum += stone
	}

	space := sum / 2
	dp := make([]int, space+1)

	for _, stone := range stones {
		for j := space; j >= stone; j-- {
			dp[j] = max(dp[j], dp[j-stone]+stone)
		}
	}

	return sum - 2*dp[space]
}
```

## [Target Sum](https://leetcode.com/problems/target-sum/description/)

`left + right = sum; left - right = target; left = (sum + target) / 2`

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (Math.abs(target) > sum) {
            return 0;
        }
        if ((sum + target) % 2 == 1) {
            return 0;
        }
        int space = (sum + target) / 2;
        int[] dp = new int[space + 1];

        dp[0] = 1;
        for (int i = 0; i < nums.length; i++) {
            for (int j = space; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]];
            }
        }

        return dp[space];
    }
}
```

```go
func findTargetSumWays(nums []int, target int) int {
	sum := 0
	for _, num := range nums {
		sum += num
	}

	if abs(target) > sum {
		return 0
	}
	if (target+sum)%2 == 1 {
		return 0
	}

	space := (target + sum) / 2
	dp := make([]int, space+1)
	dp[0] = 1
	for _, num := range nums {
		for j := space; j >= num; j-- {
			dp[j] += dp[j-num]
		}
	}
	return dp[space]
}

func abs(x int) int {
	return int(math.Abs(float64(x)))
}
```

## [Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/description/)

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        for (String str : strs) {
            int oneNum = 0, zeroNum = 0;
            for (char c : str.toCharArray()) {
                if (c == '0') {
                    zeroNum++;
                } else {
                    oneNum++;
                }
            }
            for (int i = m; i >= zeroNum; i--) {
                for (int j = n; j >= oneNum; j--) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
                }
            }
        }
        return dp[m][n];
    }
}
```

```go
func findMaxForm(strs []string, m int, n int) int {
	dp := make([][]int, m+1)
	for i, _ := range dp {
		dp[i] = make([]int, n+1)
	}

	for i := 0; i < len(strs); i++ {
		zeroNum, oneNum := 0, 0
		for _, v := range strs[i] {
			if v == '0' {
				zeroNum++
			} else {
				oneNum++
			}
		}

		for j := m; j >= zeroNum; j-- {
			for k := n; k >= oneNum; k-- {
				dp[j][k] = max(dp[j][k], dp[j-zeroNum][k-oneNum]+1)
			}
		}
	}
	return dp[m][n]
}
```

