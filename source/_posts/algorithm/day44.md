---
title: Day 44
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 45 Monotonic stack
data: 2025-04-15
---

# Day 44

## [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/description/)

```java
class Solution {
    public int trap(int[] height) {
        Deque<Integer> stack = new LinkedList<>();
        int res = 0;
        stack.push(0);
        for (int i = 1; i < height.length; i++) {
            if (height[i] < height[stack.peek()]) {
                stack.push(i);
            } else if (height[i] == height[stack.peek()]) {
                stack.pop();
                stack.push(i);
            } else {
                while (!stack.isEmpty() && height[i] > height[stack.peek()]) {
                    int mid = stack.pop();
                    if (!stack.isEmpty()) {
                        int left = stack.peek();
                        int h = Math.min(height[left], height[i]) - height[mid];
                        int w = i - left - 1;
                        res += h * w;
                    }
                }
                stack.push(i);
            }
        }
        return res;
    }
}
```

```go
func trap(height []int) int {
	stack := []int{0}
	res := 0
	for i, v := range height {
		if v < height[stack[len(stack)-1]] {
			stack = append(stack, i)
		} else if v == height[stack[len(stack)-1]] {
			stack = stack[:len(stack)-1]
			stack = append(stack, i)
		} else {
			for len(stack) != 0 && v > height[stack[len(stack)-1]] {
				mid := stack[len(stack)-1]
				stack = stack[:len(stack)-1]
				if len(stack) != 0 {
					left := stack[len(stack)-1]
					h, w := min(height[left], v)-height[mid], i-left-1
					res += h * w
				}
			}
			stack = append(stack, i)
		}
	}
	return res
}

func min(a, b int) int {
	if a > b {
		return b
	}
	return a
}
```

## [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)

```java
class Solution {
    int largestRectangleArea(int[] heights) {
        Deque<Integer> stack = new LinkedList<>();
        
        int [] newHeights = new int[heights.length + 2];
        newHeights[0] = 0;
        newHeights[newHeights.length - 1] = 0;
        for (int index = 0; index < heights.length; index++){
            newHeights[index + 1] = heights[index];
        }

        heights = newHeights;
        
        stack.push(0);
        int result = 0;
        for (int i = 1; i < heights.length; i++) {
            if (heights[i] > heights[stack.peek()]) {
                stack.push(i);
            } else if (heights[i] == heights[stack.peek()]) {
                stack.pop();
                stack.push(i);
            } else {
                while (heights[i] < heights[stack.peek()]) {
                    int mid = stack.peek();
                    stack.pop();
                    int left = stack.peek();
                    int right = i;
                    int w = right - left - 1;
                    int h = heights[mid];
                    result = Math.max(result, w * h);
                }
                stack.push(i);
            }
        }
        return result;
    }
}
```

```go
func largestRectangleArea(heights []int) int {
	result := 0
	heights = append([]int{0}, heights...)
	heights = append(heights, 0)
	stack := []int{0}

	for i := 1; i < len(heights); i++ {
		if heights[i] > heights[stack[len(stack)-1]] {
			stack = append(stack, i)
		} else if heights[i] == heights[stack[len(stack)-1]] {
			stack = stack[:len(stack)-1]
			stack = append(stack, i)
		} else {
			for len(stack) > 0 && heights[i] < heights[stack[len(stack)-1]] {
				mid := stack[len(stack)-1]
				stack = stack[:len(stack)-1]
				if len(stack) > 0 {
					left := stack[len(stack)-1]
					right := i
					w := right - left - 1
					h := heights[mid]
					result = max(result, w*h)
				}
			}
			stack = append(stack, i)
		}
	}
	return result
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

