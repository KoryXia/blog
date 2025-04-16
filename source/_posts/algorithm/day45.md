---
title: Day 45
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 45 Array
data: 2025-04-16
---

# Day 45

## [Binary Search](https://leetcode.com/problems/binary-search/description/)

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        while(left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] > target) right = mid;
            else if (nums[mid] < target) left = mid + 1;
            else return mid;
        }
        return -1;
    }
}
```

```go
func search(nums []int, target int) int {
	left, right := 0, len(nums)
	for left < right {
		mid := left + ((right - left) >> 1)
		if nums[mid] > target {
			right = mid
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			return mid
		}
	}
	return -1
}
```

## [Remove Element](https://leetcode.com/problems/remove-element/description/)

**Fast pointer**: Iterates through each element in the array.

**Slow pointer**: Indicates the position where the next valid (non-`val`) element should be placed.

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int slow = 0;
        for (int fast = 0; fast < nums.length; fast++) {
            if (nums[fast] != val) {
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }
}
```

```go
func removeElement(nums []int, val int) int {
	slow := 0
	for _, v := range nums {
		if v != val {
			nums[slow] = v
			slow++
		}
	}
	return slow
}
```

## [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] res = new int[nums.length];
        int i = nums.length - 1;
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            if (nums[left] * nums[left] < nums[right] * nums[right]) {
                res[i] = nums[right] * nums[right];
                right--;
                i--;
            } else {
                res[i] = nums[left] * nums[left];
                left++;
                i--;
            }
        }
        return res;
    }
}
```

```go
func sortedSquares(nums []int) []int {
	left, right, i := 0, len(nums)-1, len(nums)-1
	res := make([]int, len(nums))

	for left <= right {
		squareL, squareR := nums[left]*nums[left], nums[right]*nums[right]
		if squareL < squareR {
			res[i] = squareR
			right--
		} else {
			res[i] = squareL
			left++
		}
		i--
	}
	return res
}
```

## [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int res = nums.length + 1;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            while(sum >= target) {
                res = Math.min(res, i - left + 1);
                sum -= nums[left];
                left++;
            }
        }
        return res == nums.length + 1 ? 0 : res;
    }
}
```

```go
func minSubArrayLen(target int, nums []int) int {
	left, sum, res := 0, 0, len(nums)+1

	for i := 0; i < len(nums); i++ {
		sum += nums[i]
		for sum >= target {
			res = min(res, i-left+1)
			sum -= nums[left]
			left++
		}
		right++
	}

	if res == len(nums)+1 {
		return 0
	} else {
		return res
	}
}
```

