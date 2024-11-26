---
title: Day 2
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 2 Array 2
data: 2024-09-19
---

# Day 2

## [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)

Two pointer method. Right points to the end of subarray, left points to the start of subarray.

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int right = 0;
        int sum = 0;
        int minLength = nums.length + 1;
        while (right < nums.length) {
            sum += nums[right];
            while (sum >= target) {
                minLength = Math.min(minLength, right - left + 1);
                sum -= nums[left];
                left++;
            }
            right++;
        }

        return minLength == nums.length + 1 ? 0 : minLength;
    }
}
```

```go
func minSubArrayLen(target int, nums []int) int {
	left, right, sum, res := 0, 0, 0, math.MaxInt32

	for right < len(nums) {
		sum += nums[right]
		for sum >= target {
			res = min(res, right-left+1)
			sum -= nums[left]
			left++
		}
		right++
	}

	if res == math.MaxInt32 {
		return 0
	} else {
		return res
	}
}
```

## [Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/description/)

Each iteration follows left close and right open operation.

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int startX = 0;
        int startY = 0;
        int offset = 1;
        int count = 1;
        int i = 0;
        int j = 0;
        int loop = 1;
        int[][] res = new int[n][n];
        while (loop <= n / 2) {
            i = startX;
            j = startY;
            for (j = startY; j < n - offset; j++) {
                res[i][j] = count++;
            }

            for (i = startX; i < n - offset; i++) {
                res[i][j] = count++;
            }

            for (; j > startY; j--) {
                res[i][j] = count++;
            }

            for (; i > startX; i--) {
                res[i][j] = count++;
            }
            startX++;
            startY++;
            offset++;
            loop++;
        }

        if (n % 2 == 1){
            res[n / 2][n / 2] = count;
        }
        return res;
    }
}
```

```go
func generateMatrix(n int) [][]int {
	startx, starty := 0, 0
	var loop int = n / 2
	var center int = n / 2
	count := 1
	offset := 1
	res := make([][]int, n)
	for i := 0; i < n; i++ {
		res[i] = make([]int, n)
	}
	for loop > 0 {
		i, j := startx, starty

		for j = starty; j < n-offset; j++ {
			res[i][j] = count
			count++
		}

		for i = startx; i < n-offset; i++ {
			res[i][j] = count
			count++
		}

		for ; j > starty; j-- {
			res[i][j] = count
			count++
		}

		for ; i > startx; i-- {
			res[i][j] = count
			count++
		}
		startx++
		starty++
		offset++
		loop--
	}
	if n%2 == 1 {
		res[center][center] = n * n
	}
	return res
}
```

## The sum of interval

Given a integer array, calculate the the sum of given interval.
Use prefix sum array to solve this problem.

```java
public class Solution {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int n = scanner.nextInt();
        int[] vec = new int[n];
        int[] p = new int[n];

        int presum = 0;
        for (int i = 0; i < n; i++) {
            vec[i] = scanner.nextInt();
            presum += vec[i];
            p[i] = presum;
        }

        while (scanner.hasNextInt()) {
            int a = scanner.nextInt();
            int b = scanner.nextInt();

            int sum;
            if (a == 0) {
                sum = p[b];
            } else {
                sum = p[b] - p[a - 1];
            }
            System.out.println(sum);
        }

        scanner.close();
    }
}
```

# Classical method for array like problem

1. Binary search
2. Two pointers
3. Sliding window
4. Simulation
5. Prefix sum
