---
title: Day 9
tags: Algorithm
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 9 Stack & Queue section 1
---

# Day 9

## [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)

```java
class MyQueue {
    private Stack<Integer> inStack;
    private Stack<Integer> outStack;
    public MyQueue() {
        this.outStack = new Stack<>();
        this.inStack = new Stack<>();
    }
    
    public void push(int x) {
        inStack.push(x);
    }
    
    public int pop() {
        if (outStack.isEmpty()) {
            in2out();
        }
        return outStack.pop();
    }
    
    public int peek() {
        if (outStack.isEmpty()) {
            in2out();
        }
        return outStack.peek();
    }

    public void in2out() {
        while (!inStack.isEmpty()) {
            outStack.push(inStack.pop());
        }
    }
    
    public boolean empty() {
        return inStack.isEmpty() && outStack.isEmpty();
    }
}
```

```go
type MyStack []int

func (s *MyStack) Push(v int) {
	*s = append(*s, v)
}

func (s *MyStack) Pop() int {
	val := (*s)[len(*s)-1]
	*s = (*s)[:len(*s)-1]
	return val
}

func (s *MyStack) Peek() int {
	return (*s)[len(*s)-1]
}

func (s *MyStack) Size() int {
	return len(*s)
}

func (s *MyStack) Empty() bool {
	return s.Size() == 0
}

type MyQueue struct {
	stackIn  *MyStack
	stackOut *MyStack
}

func Constructor() MyQueue {
	return MyQueue{
		stackIn:  &MyStack{},
		stackOut: &MyStack{},
	}
}

func (this *MyQueue) Push(x int) {
	this.stackIn.Push(x)
}

func (this *MyQueue) Pop() int {
	this.fillStackOut()
	return this.stackOut.Pop()
}

func (this *MyQueue) Peek() int {
	this.fillStackOut()
	return this.stackOut.Peek()
}

func (this *MyQueue) Empty() bool {
	return this.stackIn.Empty() && this.stackOut.Empty()
}

func (this *MyQueue) fillStackOut() {
	if this.stackOut.Empty() {
		for !this.stackIn.Empty() {
			val := this.stackIn.Pop()
			this.stackOut.Push(val)
		}
	}
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
		queue: make([]int, 0),
	}
}

func (this *MyStack) Push(x int) {
	this.queue = append(this.queue, x)
}

func (this *MyStack) Pop() int {
	n := len(this.queue) - 1
	for n != 0 {
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
	this.queue = append(this.queue, val)
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
        Map<Character, Character> map = new HashMap<>();
        map.put(')', '(');
        map.put(']', '[');
        map.put('}', '{');

        for (Character c : s.toCharArray()) {
            if (map.containsKey(c)) {
                if (stack.isEmpty() || stack.pop() != map.get(c)) {
                    return false;
                }
                continue;
            } else {
                stack.push(c);
            }
        }
        return stack.isEmpty();
    }
}
```

```go
func isValid(s string) bool {
	stack := make([]rune, 0)

	m := make(map[rune]rune)
	m[')'] = '('
	m[']'] = '['
	m['}'] = '{'

	for _, c := range s {
		if c == '(' || c == '[' || c == '{' {
			stack = append(stack, c)
		} else {
			if len(stack) == 0 {
				return false
			}
			peek := stack[len(stack)-1]
			if peek != m[c] {
				return false
			}
			stack = stack[:len(stack)-1]
		}
	}
	return len(stack) == 0
}
```

## [Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)

```java
class Solution {
    public String removeDuplicates(String s) {
        StringBuffer res = new StringBuffer();
        int curr = -1;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (curr >= 0 && res.charAt(curr) == c) {
                res.deleteCharAt(curr);
                curr--;
            } else {
                res.append(c);
                curr++;
            }
        }
        return res.toString();
    }
}
```

```go
func removeDuplicates(s string) string {
	stack := make([]byte, 0)
	for i := 0; i < len(s); i++ {
		if len(stack) > 0 && stack[len(stack)-1] == s[i] {
			stack = stack[:len(stack)-1]
		} else {
			stack = append(stack, s[i])
		}
	}
	return string(stack)
}
```

