---
title: Day 54
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 54 Stack & Queue
data: 2025-05-05
---

# Day 54

## [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)

```java
class MyQueue {
    Stack<Integer> inStack;
    Stack<Integer> outStack;

    public MyQueue() {
        this.inStack = new Stack<>();
        this.outStack = new Stack<>();
    }

    public void push(int x) {
        inStack.push(x);
    }

    public int pop() {
        in2out();
        return outStack.pop();
    }

    public int peek() {
        in2out();
        return outStack.peek();
    }

    public boolean empty() {
        return inStack.isEmpty() && outStack.isEmpty();
    }

    private void in2out() {
        if (!outStack.isEmpty())
            return;
        while (!inStack.isEmpty()) {
            outStack.push(inStack.pop());
        }
    }
}
```

```go
type Stack []int

func (s *Stack) Empty() bool {
	return len(*s) == 0
}

func (s *Stack) Push(x int) {
	*s = append(*s, x)

}

func (s *Stack) Pop() int {
	val := (*s)[len(*s)-1]
	*s = (*s)[:len(*s)-1]
	return val
}

func peek(s *Stack) int {
	return (*s)[len(*s)-1]
}

```

## [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/description/)

```java
class MyStack {
    private Queue<Integer> queue;

    public MyStack() {
        this.queue = new LinkedList<Integer>();
    }
    
    public void push(int x) {
        this.queue.offer(x);
        for (int i = 1; i < this.queue.size(); i++) {
            this.queue.offer(this.queue.poll());
        }
    }
    
    public int pop() {
        return this.queue.poll();
    }
    
    public int top() {
        return this.queue.peek();
    }
    
    public boolean empty() {
        return this.queue.isEmpty();
    }
}
```

```go
type MyStack struct {
	queue []int
}

func Constructor() MyStack {
	return MyStack{
		queue: []int{},
	}
}

func (this *MyStack) Push(x int) {
	this.queue = append(this.queue, x)
}

func (this *MyStack) Pop() int {
	n := len(this.queue) - 1
	for n > 0 {
		val := this.queue[0]
		this.queue = this.queue[1:]
		this.queue = append(this.queue, val)
		n--
	}
	val := this.queue[0]
	this.queue = this.queue[1:]
	return val
}

func (this *MyStack) Top() int {
	val := this.Pop()
	this.Push(val)
	return val
}

func (this *MyStack) Empty() bool {
	return len(this.queue) == 0
}
```

## [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (char c : s.toCharArray()) {
            if (c == ')' && !stack.isEmpty() && stack.peek() == '(') {
                stack.pop();
            } else if (c == '}' && !stack.isEmpty() && stack.peek() == '{') {
                stack.pop();
            } else if (c == ']' && !stack.isEmpty() && stack.peek() == '[') {
                stack.pop();
            } else {
                stack.push(c);
            }
        }
        return stack.size() == 0;
    }
}
```

```go
func isValid(s string) bool {
	var stack []rune
	for _, c := range s {
		if c == '(' || c == '[' || c == '{' {
			stack = append(stack, c)
		} else {
			if len(stack) == 0 {
				return false
			}
			prev := stack[len(stack)-1]
			if prev == '(' && c == ')' {
				stack = stack[:len(stack)-1]
			} else if prev == '[' && c == ']' {
				stack = stack[:len(stack)-1]
			} else if prev == '{' && c == '}' {
				stack = stack[:len(stack)-1]
			} else {
				return false
			}
		}
	}
	return len(stack) == 0
}
```

