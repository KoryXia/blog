---
title: Day 24
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 24 Greedy
data: 2024-12-09
---

# Day 24

## [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int res = 0;
        for (int i = 1; i < prices.length; i++) {
            res += Math.max(prices[i] - prices[i - 1], 0);
        }
        return res;
    }
}
```

```go
func maxProfit(prices []int) int {
	res := 0
	for i := 1; i < len(prices); i++ {
		profit := prices[i] - prices[i-1]
		if profit > 0 {
			res += profit
		}
	}
	return res
}
```

## [Jump Game](https://leetcode.com/problems/jump-game/description/)

Use range coverage method

```java
class Solution {
    public boolean canJump(int[] nums) {
        int cover = 0;
        for (int i = 0; i <= cover; i++) {
            cover = Math.max(i + nums[i], cover);
            if (cover >= nums.length - 1) {
                return true;
            }
        }
        return false;
    }
}
```

```go
func canJump(nums []int) bool {
	cover := 0
	for i := 0; i <= cover; i++ {
		if i+nums[i] > cover {
			cover = i + nums[i]
		}
		if cover >= len(nums)-1 {
			return true
		}
	}
	return false
}
```

## [Jump Game II](https://leetcode.com/problems/jump-game-ii/description/)

```java
class Solution {
    public int jump(int[] nums) {
        if (nums == null || nums.length == 0 || nums.length == 1) {
            return 0;
        }
        int currCover = 0;
        int maxCover = 0;
        int res = 0;
        for (int i = 0; i < nums.length; i++) {
            maxCover = Math.max(i + nums[i], maxCover);
            if (maxCover >= nums.length - 1) {
                res++;
                break;
            }

            if (i == currCover) {
                currCover = maxCover;
                res++;
            }
        }
        return res;
    }
}
```

```go
func jump(nums []int) int {
	if nums == nil || len(nums) == 0 || len(nums) == 1 {
		return 0
	}

	res, maxCover, currCover := 0, 0, 0
	for i := 0; i < len(nums); i++ {
		if (i + nums[i]) > maxCover {
			maxCover = i + nums[i]
		}

		if maxCover >= (len(nums) - 1) {
			res++
			break
		}

		if i == currCover {
			currCover = maxCover
			res++
		}
	}
	return res
}
```

