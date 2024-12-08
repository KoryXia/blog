---
title: Day 23
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 23 Greedy
data: 2024-12-08
---

# Day 23

## [Assign Cookies](https://leetcode.com/problems/assign-cookies/description/)

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int cookieIndex = s.length - 1;
        int res = 0;
        for (int i = g.length - 1; i >= 0 && cookieIndex >= 0; i--) {
            if (g[i] <= s[cookieIndex]) {
                cookieIndex--;
                res++;
            }
        }
        return res;
    }
}
```

```go

func findContentChildren(g []int, s []int) int {
	sort.Ints(g)
	sort.Ints(s)
	cookieIndex, res := len(s)-1, 0
	for i := len(g) - 1; i >= 0 && cookieIndex >= 0; i-- {
		if g[i] <= s[cookieIndex] {
			cookieIndex--
			res++
		}
	}
	return res
}

```

## [Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/description/)

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length == 1) {
            return 1;
        }
        int prediff = 0;
        int currdiff = 0;
        int res = 1;
        for (int i = 0; i < nums.length - 1; i++) {
            currdiff = nums[i + 1] - nums[i];
            if ((prediff >= 0 && currdiff < 0) || (prediff <= 0 && currdiff > 0)) {
                res++;
                prediff = currdiff;
            }
        }
        return res;
    }
}
```

```go
func wiggleMaxLength(nums []int) int {
	if len(nums) == 1 {
		return 1
	}
	prediff, currdiff, res := 0, 0, 1
	for i := 0; i < len(nums)-1; i++ {
		currdiff = nums[i+1] - nums[i]
		if (prediff >= 0 && currdiff < 0) || (prediff <= 0 && currdiff > 0) {
			res++
			prediff = currdiff
		}
	}
	return res
}
```

## [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        int res = Integer.MIN_VALUE;
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            res = Math.max(res, sum);
            if (sum < 0) {
                sum = 0;
            }
        }
        return res;
    }
}
```

```go
func maxSubArray(nums []int) int {
	sum, res := 0, nums[0]
	for i := 0; i < len(nums); i++ {
		sum += nums[i]
		if sum > res {
			res = sum
		}
		if sum < 0 {
			sum = 0
		}
	}
	return res
}
```

