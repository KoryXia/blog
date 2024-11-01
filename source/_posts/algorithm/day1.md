---
title: Day 1
tags: Algorithm
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 1 Array 1
---

# Day 1

## [Binary Search](https://leetcode.com/problems/binary-search/)

To apply binary search, the array needs to satisfy two prerequisites: The array is ordered, the elements are unique.

There are two solutions for this problem：

**left right close interval：**

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) {
                right = mid - 1;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
}
```

```go
func search(nums []int, target int) int {
	left := 0
	right := len(nums) - 1

	for left <= right {
		mid := left + (right-left)/2

		if nums[mid] > target {
			right = mid - 1
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			return mid
		}
	}
	return -1
}
```

**left close right open interval:**

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length;

        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) {
                right = mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
}
```

```go
func search(nums []int, target int) int {
	left := 0
	right := len(nums)

	for left < right {
		mid := left + (right-left)/2

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

## [Remove element](https://leetcode.com/problems/remove-element/description/)

Slow and fast pointer method can solve this problem.

Fast: next element

Slow: index of the new array

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int slow = 0;
        for (int fast = 0; fast < nums.length; fast++) {
            if (nums[fast] != val) {
                nums[slow++] = nums[fast];
            }
        }
        return slow;
    }
}
```

```go
func removeElement(nums []int, val int) int {
	slow := 0
	for _, num := range nums {
		if num != val {
			nums[slow] = num
			slow++
		}
	}
	return slow
}
```

## [Squares of a sorted array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)

Use two pointer from the left and right side of array.

The array is sorted, the larger square value must come from left (possibly negative) or the right. Each iteration
compares values from the left and right sides and update the larger value to the end of the result array.

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] res = new int[nums.length];
        int i = nums.length - 1;
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            if (Math.abs(nums[left]) > Math.abs(nums[right])) {
                res[i--] = nums[left] * nums[left];
                left++;
            } else {
                res[i--] = nums[right] * nums[right];
                right--;
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
		squareLeft, squareRight := nums[left]*nums[left], nums[right]*nums[right]
		if squareLeft > squareRight {
			res[i] = squareLeft
			left++
		} else {
			res[i] = squareRight
			right--
		}
		i--
	}
	return res
}
```

