---
title: Day 25
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 25 Greedy
data: 2024-12-10
---

# Day 25

## [Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/)

```java
class Solution {
    public int largestSumAfterKNegations(int[] nums, int K) {
        nums = IntStream
        .of(nums)
        .boxed()
        .sorted((o1, o2) -> Math.abs(o2) - Math.abs(o1))
        .mapToInt(Integer::intValue)
        .toArray();

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] < 0 && K > 0) {
                nums[i] = -nums[i];
                K--;
            }
        }

        if (K % 2 == 1) {
            nums[nums.length - 1] = -nums[nums.length - 1];
        }

        return IntStream.of(nums).sum();
    }
}
```

```go
func largestSumAfterKNegations(nums []int, k int) int {
	sort.Slice(nums, func(i, j int) bool {
		return math.Abs(float64(nums[i])) > math.Abs(float64(nums[j]))
	})

	for i := 0; i < len(nums); i++ {
		if k > 0 && nums[i] < 0 {
			nums[i] = -nums[i]
			k--
		}
	}

	if k%2 == 1 {
		nums[len(nums)-1] = -nums[len(nums)-1]
	}

	result := 0
	for i := 0; i < len(nums); i++ {
		result += nums[i]
	}
	return result
}
```

## [Gas Station](https://leetcode.com/problems/gas-station/description/)

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int currSum = 0;
        int totalSum = 0;
        int start = 0;
        for (int i = 0; i < gas.length; i++) {
            currSum += gas[i] - cost[i];
            totalSum +=  gas[i] - cost[i];
            if (currSum < 0) {
                start = i + 1;
                currSum = 0;
            }
        }

        if (totalSum < 0) {
            return -1;
        }

        return start;
    }
}
```

```go
func canCompleteCircuit(gas []int, cost []int) int {
	currSum, totalSum, start := 0, 0, 0
	for i := 0; i < len(gas); i++ {
		currSum += gas[i] - cost[i]
		totalSum += gas[i] - cost[i]
		if currSum < 0 {
			start = i + 1
			currSum = 0
		}
	}
	if totalSum < 0 {
		return -1
	}
	return start
}
```

## [Candy](https://leetcode.com/problems/candy/description/)

```java
class Solution {
    public int candy(int[] ratings) {
        int[] candy = new int[ratings.length];
        candy[0] = 1;
        for (int i = 1; i < ratings.length; i++) {
            candy[i] = (ratings[i] > ratings[i - 1]) ? candy[i - 1] + 1 : 1;
        }

        for (int i = ratings.length - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candy[i] = Math.max(candy[i], candy[i + 1] + 1);
            }
        }

        int res = 0;
        for (int i = 0; i < candy.length; i++) {
            res += candy[i];
        }
        return res;
    }
}
```

```go
func candy(ratings []int) int {
	candy := make([]int, len(ratings))
	sum := 0
	for i := 0; i < len(ratings); i++ {
		candy[i] = 1
	}

	for i := 0; i < len(ratings)-1; i++ {
		if ratings[i] < ratings[i+1] {
			candy[i+1] = candy[i] + 1
		}
	}

	for i := len(ratings) - 1; i > 0; i-- {
		if ratings[i-1] > ratings[i] {
			candy[i-1] = findMax(candy[i-1], candy[i]+1)
		}
	}
	for i := 0; i < len(ratings); i++ {
		sum += candy[i]
	}
	return sum
}

func findMax(num1 int, num2 int) int {
	if num1 > num2 {
		return num1
	}
	return num2
}
```

