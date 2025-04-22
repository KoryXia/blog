---
title: Day 47
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 47 Linkedlist
data: 2025-04-22
---

# Day 47

## [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/description/)

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode();
        dummy.next = head;
        ListNode curr = dummy;
        while (curr.next != null) {
            if (curr.next.val == val) {
                curr.next = curr.next.next;
            } else {
                curr = curr.next;
            }
        }
        return dummy.next;
    }
}
```

```go
func removeElements(head *ListNode, val int) *ListNode {
	dummy := &ListNode{}
	dummy.Next = head
	curr := dummy
	for curr.Next != nil {
		if curr.Next.Val == val {
			curr.Next = curr.Next.Next
		} else {
			curr = curr.Next
		}
	}
	return dummy.Next
}
```

# [Design Linked List](https://leetcode.com/problems/design-linked-list/description/)

```java
class MyLinkedList {
    int size;
    ListNode head;
    ListNode tail;

    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
        tail = new ListNode(0);
        head.next = tail;
        tail.prev = head;
    }

    public int get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }

        ListNode curr = head;
        if (index <= size / 2) {
            for (int i = 0; i <= index; i++) {
                curr = curr.next;
            }
        } else {
            curr = tail;
            for (int i = 0; i < size - index; i++) {
                curr = curr.prev;
            }
        }
        return curr.val;
    }

    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    public void addAtIndex(int index, int val) {
        if (index < 0 || index > size) {
            return;
        }
        ListNode pre = head;
        for (int i = 0; i < index; i++) {
            pre = pre.next;
        }
        ListNode node = new ListNode(val);
        node.next = pre.next;
        pre.next.prev = node;
        node.prev = pre;
        pre.next = node;
        size++;
    }

    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        ListNode pre = head;
        for (int i = 0; i < index; i++) {
            pre = pre.next;
        }
        pre.next = pre.next.next;
        pre.next.prev = pre;
        size--;;
    }
}

class ListNode {
    int val;
    ListNode next;
    ListNode prev;

    public ListNode(int val) {
        this.val = val;
    }
}
```

```go
type Node struct {
	Val  int
	Next *Node
	Prev *Node
}

type MyLinkedList struct {
	head *Node
	tail *Node
	size int
}

func Constructor() MyLinkedList {
	tail := &Node{
		Val:  0,
		Next: nil,
		Prev: nil,
	}
	head := &Node{
		Val:  0,
		Next: nil,
		Prev: nil,
	}
	head.Next = tail
	tail.Prev = head
	return MyLinkedList{
		head: head,
		tail: tail,
		size: 0,
	}
}

func (this *MyLinkedList) Get(index int) int {
	if index < 0 || index >= this.size {
		return -1
	}

	node := this.head
	if index <= this.size/2 {
		for i := 0; i <= index; i++ {
			node = node.Next
		}
	} else {
		node = this.tail
		for i := 0; i < this.size-index; i++ {
			node = node.Prev
		}
	}
	return node.Val
}

func (this *MyLinkedList) AddAtHead(val int) {
	this.AddAtIndex(0, val)
}

func (this *MyLinkedList) AddAtTail(val int) {
	this.AddAtIndex(this.size, val)
}

func (this *MyLinkedList) AddAtIndex(index int, val int) {
	if index < 0 || index > this.size {
		return
	}

	prev := this.head
	for i := 0; i < index; i++ {
		prev = prev.Next
	}

	node := &Node{
		Val:  val,
		Next: nil,
		Prev: nil,
	}
	node.Next = prev.Next
	prev.Next.Prev = node
	node.Prev = prev
	prev.Next = node
	this.size++
}

func (this *MyLinkedList) DeleteAtIndex(index int) {
	if index < 0 || index >= this.size {
		return
	}

	prev := this.head
	for i := 0; i < index; i++ {
		prev = prev.Next
	}
	prev.Next.Next.Prev = prev
	prev.Next = prev.Next.Next
	this.size--
}
```

## [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        
        while (head != null) {
            ListNode next = head.next;
            head.next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }
}
```

```go
func reverseList(head *ListNode) *ListNode {
	var prev *ListNode

	for head != nil {
		next := head.Next
		head.Next = prev
		prev = head
		head = next
	}
	return prev
}
```

## [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode next = head.next;
        ListNode node = swapPairs(next.next);

        head.next = node;
        next.next = head;
        return next;
    }
}
```

```go
func swapPairs(head *ListNode) *ListNode {
	if head == nil || head.Next == nil {
		return head
	}
	next := head.Next

	node := swapPairs(next.Next)

	next.Next = head
	head.Next = node

	return next
}
```

