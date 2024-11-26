---
title: Day 4
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 4 LinkedList 2
data: 2024-09-23
---

# Day 4

## [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode cur = dummy;
        
        while (cur.next != null && cur.next.next != null) {
            ListNode temp = cur.next;
            ListNode next = cur.next.next.next;
            cur.next = cur.next.next; // Step 1
            cur.next.next = temp; // Step 2
            cur.next.next.next = next; // Step 3
            cur = cur.next.next; // move cur pointer
        }
        return dummy.next;
    }
}
```

```go
func swapPairs(head *ListNode) *ListNode {
	dummy := &ListNode{
		Next: head,
	}
	cur := dummy

	for cur.Next != nil && cur.Next.Next != nil {
		temp := cur.Next
		next := cur.Next.Next.Next
		cur.Next = cur.Next.Next
		cur.Next.Next = temp
		cur.Next.Next.Next = next
		cur = cur.Next.Next
	}
	return dummy.Next
}
```

## [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

Using fast / slow pointer to solve this problem. Fast pointer should move `n + 1` steps ahead.

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null) {
            return head;
        }
        ListNode dummy = new ListNode();
        dummy.next = head;
        ListNode slow = dummy;
        ListNode fast = dummy;
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }
        
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;   
        }
        slow.next = slow.next.next;
        return dummy.next;
    }
}
```

```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
	if head == nil {
		return head
	}

	dummy := &ListNode{0, head}
	slow, fast := dummy, dummy
	for i := 0; i <= n; i++ {
		fast = fast.Next
	}

	for fast != nil {
		slow = slow.Next
		fast = fast.Next
	}

	slow.Next = slow.Next.Next
	return dummy.Next
}
```

## [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p1 = headA;
        ListNode p2 = headB;
        
        while (p1 != p2) {
            p1 = p1 == null ? headB : p1.next;
            p2 = p2 == null ? headA : p2.next;
        }
        return p1;
    }
}
```

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
	p1, p2 := headA, headB

	for p1 != p2 {
		if p1 != nil {
			p1 = p1.Next
		} else {
			p1 = headB
		}

		if p2 != nil {
			p2 = p2.Next
		} else {
			p2 = headA
		}
	}
	return p1
}
```

##  [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                ListNode node1 = fast;
                ListNode node2 = head;
                while (node1 != node2) {
                    node1 = node1.next;
                    node2 = node2.next;
                }
                return node1;
            }
        }
        return null;
    }
}
```

```go
func detectCycle(head *ListNode) *ListNode {
	slow, fast := head, head

	for fast != nil && fast.Next != nil {
		slow = slow.Next
		fast = fast.Next.Next
		if slow == fast {
			slow = head
			for slow != fast {
				slow = slow.Next
				fast = fast.Next
			}
			return slow
		}
	}
	return nil
}
```

