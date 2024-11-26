---
title: Day 10
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 10 Stack & Queue section 2
data: 2024-10-31
---

# Day 10

## [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();

        for (String token : tokens) {
            if (isInteger(token)) {
                stack.push(Integer.valueOf(token));
            } else {
                int num1 = stack.pop();
                int num2 = stack.pop();
                switch (token) {
                    case "+":
                        stack.push(num2 + num1);
                        break;
                    case "-":
                        stack.push(num2 - num1);
                        break;
                    case "*":
                        stack.push(num2 * num1);
                        break;
                    case "/":
                        stack.push(num2 / num1);
                        break;
                    default:
                        break;
                }
            }
        }
        return stack.pop();
    }

    public boolean isInteger(String s) {
        try {
            Integer.parseInt(s);
            return true;
        } catch (NumberFormatException e) {
            return false;
        }
    }
}
```

```go
func evalRPN(tokens []string) int {
	stack := []int{}
	for _, token := range tokens {
		val, err := strconv.Atoi(token)
		if err == nil {
			stack = append(stack, val)
		} else {
			num1, num2 := stack[len(stack)-2], stack[len(stack)-1]
			stack = stack[:len(stack)-2]
			switch token {
			case "+":
				stack = append(stack, num1+num2)
			case "-":
				stack = append(stack, num1-num2)
			case "*":
				stack = append(stack, num1*num2)
			case "/":
				stack = append(stack, num1/num2)
			}
		}
	}
	return stack[0]
}
```

## [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/)

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        Queue<int[]> queue = new PriorityQueue<>(new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0] != o2[0] ? o2[0] - o1[0] : o2[1] - o1[1];
            }

        });

        for (int i = 0; i < k; i++) {
            queue.add(new int[] { nums[i], i });
        }
        int[] res = new int[nums.length - k + 1];
        res[0] = queue.peek()[0];
        for (int i = k; i < nums.length; i++) {
            queue.add(new int[] { nums[i], i });
            while (queue.peek()[1] < i - k + 1) {
                queue.poll();
            }
            res[i - k + 1] = queue.peek()[0];
        }
        return res;
    }
}
```

```go
type MyQueue struct {
	queue []int
}

func NewMyQueue() *MyQueue {
	return &MyQueue{
		queue: make([]int, 0),
	}
}

func (m *MyQueue) Front() int {
	return m.queue[0]
}

func (m *MyQueue) Back() int {
	return m.queue[len(m.queue)-1]
}

func (m *MyQueue) Empty() bool {
	return len(m.queue) == 0
}

func (m *MyQueue) Push(val int) {
	for !m.Empty() && val > m.Back() {
		m.queue = m.queue[:len(m.queue)-1]
	}
	m.queue = append(m.queue, val)
}

func (m *MyQueue) Pop(val int) {
	if !m.Empty() && val == m.Front() {
		m.queue = m.queue[1:]
	}
}

func maxSlidingWindow(nums []int, k int) []int {
	queue := NewMyQueue()
	res := make([]int, 0)
	for i := 0; i < k; i++ {
		queue.Push(nums[i])
	}

	res = append(res, queue.Front())

	for i := k; i < len(nums); i++ {
		queue.Pop(nums[i-k])
		queue.Push(nums[i])
		res = append(res, queue.Front())
	}
	return res
}
```

## [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        Queue<Integer> queue = new PriorityQueue<>((a, b) -> map.get(b) - map.get(a));
        for (int key : map.keySet()) {
            queue.offer(key);
        }
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = queue.poll();
        }
        return res;
    }
}
```

```go
func topKFrequent(nums []int, k int) []int {
	res := []int{}
	mapNum := map[int]int{}
	for _, val := range nums {
		mapNum[val]++
	}
	for key, _ := range mapNum {
		res = append(res, key)
	}

	sort.Slice(res, func(a, b int) bool {
		return mapNum[res[a]] > mapNum[res[b]]
	})
	return res[:k]
}
```



