---
title: Day 55
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 55 Stack & Queue
data: 2025-05-06
---

# Day 55

## [Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)

```java
class Solution {
    public String removeDuplicates(String s) {
        StringBuilder sb = new StringBuilder();
        int top = -1;
        for (int i = 0; i < s.length(); i++) {
            if (top >= 0 && sb.charAt(top) == s.charAt(i)) {
                sb.deleteCharAt(top);
                top--;
            } else {
                sb.append(s.charAt(i));
                top++;
            }
        }
        return sb.toString();
    }
}
```

```go
func removeDuplicates(s string) string {
	var stack []rune
	for _, v := range s {
		if len(stack) > 0 && stack[len(stack)-1] == v {
			stack = stack[:len(stack)-1]
		} else {
			stack = append(stack, v)
		}
	}
	return string(stack)
}
```

## [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        for (String token : tokens) {
            if (!"+".equals(token) && !"-".equals(token) && !"*".equals(token) && !"/".equals(token)) {
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
        Queue<int[]> queue = new PriorityQueue<>((a, b) -> {
            return a[0] != b[0] ? b[0] - a[0] : b[1] - a[1];
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
type MaxQueue struct {
	queue []int
}

func NewMaxQueue() *MaxQueue {
	return &MaxQueue{
		queue: make([]int, 0),
	}
}

func (m *MaxQueue) Front() int {
	return m.queue[0]
}

func (m *MaxQueue) Back() int {
	return m.queue[len(m.queue)-1]
}

func (m *MaxQueue) Push(val int) {
	for len(m.queue) > 0 && val > m.Back() {
		m.queue = m.queue[:len(m.queue)-1]
	}
	m.queue = append(m.queue, val)
}

func (m *MaxQueue) Pop(val int) {
	if len(m.queue) > 0 && val == m.Front() {
		m.queue = m.queue[1:]
	}
}

func maxSlidingWindow(nums []int, k int) []int {
	queue := NewMaxQueue()
	res := make([]int, len(nums)-k+1)
	for i := 0; i < k; i++ {
		queue.Push(nums[i])
	}

	res[0] = queue.Front()

	for i := k; i < len(nums); i++ {
		queue.Pop(nums[i-k])
		queue.Push(nums[i])
		res[i-k+1] = queue.Front()
	}
	return res
}
```

## [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i : nums) {
            map.put(i, map.getOrDefault(i, 0) + 1);
        }
        Queue<Integer> queue = new PriorityQueue<>((a, b) -> map.get(b) - map.get(a));
        for (Integer key : map.keySet()) {
            queue.add(key);
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
	m := map[int]int{}
	for _, v := range nums {
		m[v]++
	}
	h := &IHeap{}
	heap.Init(h)
	for key, v := range m {
		heap.Push(h, [2]int{key, v})
		if h.Len() > k {
			heap.Pop(h)
		}
	}
	res := make([]int, k)
	for i := 0; i < k; i++ {
		res[k-i-1] = heap.Pop(h).([2]int)[0]
	}
	return res
}

type IHeap [][2]int

func (h *IHeap) Len() int {
	return len(*h)
}

func (h *IHeap) Less(i, j int) bool {
	return (*h)[i][1] < (*h)[j][1]
}

func (h *IHeap) Swap(i, j int) {
	(*h)[i], (*h)[j] = (*h)[j], (*h)[i]
}

func (h *IHeap) Push(x interface{}) {
	*h = append(*h, x.([2]int))
}
func (h *IHeap) Pop() interface{} {
	n := len(*h)
	v := (*h)[n-1]
	*h = (*h)[:n-1]
	return v
}

```

