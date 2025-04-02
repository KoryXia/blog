---
title: Day 36
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 3 Dynamic programming
data: 2025-04-02
---

# Day 36

## [House Robber](https://leetcode.com/problems/house-robber/description/)

```java
class Solution {
    public int rob(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);

        for (int i = 2; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[nums.length - 1];
    }
}
```

```go
func rob(nums []int) int {
	dp := make([]int, len(nums))
	dp[0] = nums[0]
	dp[1] = max(nums[0], nums[1])
	for i := 2; i < len(nums); i++ {
		dp[i] = max(dp[i-2]+nums[i], dp[i-1])
	}
	return dp[len(nums)-1]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

```

## [House Robber II](https://leetcode.com/problems/house-robber-ii/description/)

```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        int res1 = robAction(nums, 1, nums.length - 1);
        int res2 = robAction(nums, 0, nums.length - 2);
        return Math.max(res1, res2);
    }

    public int robAction(int[] nums, int start, int end) {
        if (end == start ) return nums[start];
        int[] dp = new int[nums.length];
        dp[start] = nums[start];
        dp[start + 1] = Math.max(nums[start], nums[start + 1]);

        for (int i = start + 2; i <= end; i++) {
            dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[end];
    }
}
```

```go
func rob(nums []int) int {
	if len(nums) == 1 {
		return nums[0]
	}
	res1 := robAction(nums[1:])
	res2 := robAction(nums[:len(nums)-1])
	return max(res1, res2)
}

func robAction(nums []int) int {
	if len(nums) == 1 {
		return nums[0]
	}
	dp := make([]int, len(nums))
	dp[0] = nums[0]
	dp[1] = max(nums[0], nums[1])

	for i := 2; i < len(nums); i++ {
		dp[i] = max(dp[i-2]+nums[i], dp[i-1])
	}
	return dp[len(nums)-1]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

## [House Robber III](https://leetcode.com/problems/house-robber-iii/description/)

```java
class Solution {
    public int rob(TreeNode root) {
        int[] res = robTree(root);
        return Math.max(res[0], res[1]);
    }

    public int[] robTree(TreeNode cur) {
        int[] res = new int[2];
        if (cur == null) return res;

        int[] left = robTree(cur.left);
        int[] right = robTree(cur.right);
        res[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        res[1] = cur.val + left[0] + right[0];
        return res;
    }
}
```

```go
func rob(root *TreeNode) int {
	res := robTree(root)
	return slices.Max(res)
}

func robTree(node *TreeNode) []int {
	if node == nil {
		return []int{0, 0}
	}
	left := robTree(node.Left)
	right := robTree(node.Right)

	unRobNode := slices.Max(left) + slices.Max(right)
	robNode := node.Val + left[0] + right[0]

	return []int{unRobNode, robNode}
}
```

