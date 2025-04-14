---
title: Day 43
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 43 Monotonic stack
data: 2025-04-14
---

# Day 43

## [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/)

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        Deque<Integer> stack = new LinkedList<>();
        int[] res = new int[temperatures.length];
        for (int i = 0; i < temperatures.length; i++) {
            while(!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i]) {
                int top = stack.pop();
                res[top] = i - top;
            }
            stack.push(i);
        }
        return res;
    }
}
```

```go
func dailyTemperatures(temperatures []int) []int {
	res := make([]int, len(temperatures))
	stack := make([]int, 0)
	for i, v := range temperatures {
		for len(stack) != 0 && v > temperatures[stack[len(stack)-1]] {
			top := stack[len(stack)-1]
			stack = stack[:len(stack)-1]
			res[top] = i - top
		}
		stack = append(stack, i)
	}
	return res
}
```

## [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/description/)

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums1.length; i++) {
            map.put(nums1[i], i);
        }
        int[] res = new int[nums1.length];
        Arrays.fill(res, -1);
        Deque<Integer> stack = new LinkedList<>();
        for (int i = 0; i < nums2.length; i++) {
            while (!stack.isEmpty() && nums2[i] > nums2[stack.peek()]) {
                int top = nums2[stack.pop()];
                if (map.containsKey(top)) {
                    res[map.get(top)] = nums2[i];
                }
            }
            stack.push(i);
        }
        return res;
    }
}
```

```go
func nextGreaterElement(nums1 []int, nums2 []int) []int {
	res := make([]int, len(nums1))
	m := map[int]int{}
	for i, v := range nums1 {
		m[v] = i
		res[i] = -1
	}

	stack := []int{}

	for i, v := range nums2 {
		for len(stack) != 0 && v > nums2[stack[len(stack)-1]] {
			top := nums2[stack[len(stack)-1]]
			stack = stack[:len(stack)-1]
			if _, ok := m[top]; ok {
				res[m[top]] = v
			}
		}
		stack = append(stack, i)
	}
	return res
}
```

## [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/description/)

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int size = nums.length;
        int[] res = new int[size];
        Arrays.fill(res, -1);
        Deque<Integer> stack = new LinkedList<>();
        for (int i = 0; i < 2 * size; i++) {
            while (!stack.isEmpty() && nums[i % size] > nums[stack.peek()]) {
                int top = stack.pop();
                res[top] = nums[i % size];
            }
            stack.push(i % size);
        }
        return res;
    }
}
```

```go
func nextGreaterElements(nums []int) []int {
	size := len(nums)
	res := make([]int, size)
	stack := []int{}
	for i, _ := range nums {
		res[i] = -1
	}
	for i := 0; i < 2*size; i++ {
		for len(stack) != 0 && nums[i%size] > nums[stack[len(stack)-1]] {
			top := stack[len(stack)-1]
			stack = stack[:len(stack)-1]
			res[top] = nums[i%size]
		}
		stack = append(stack, i%size)
	}
	return res
}
```

