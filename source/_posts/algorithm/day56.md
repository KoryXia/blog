---
title: Day 56
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 56 Binary tree
data: 2025-05-07
---

# Day 56

## [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            res.add(level);
        }
        return res;
    }

```

```go
func levelOrder(root *TreeNode) [][]int {
	var res [][]int
	if root == nil {
		return res
	}

	var queue []*TreeNode
	queue = append(queue, root)

	for len(queue) > 0 {
		size, level := len(queue), make([]int, 0)
		for i := 0; i < size; i++ {
			node := queue[0]
			queue = queue[1:]
			level = append(level, node.Val)
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
		}
		res = append(res, level)
	}
	return res
}
```

## [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        List<List<Integer>> levels = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null)
                    queue.offer(node.left);
                if (node.right != null)
                    queue.offer(node.right);
            }
            levels.add(level);
        }

        for (List<Integer> level : levels) {
            res.add(level.getLast());
        }
        return res;
    }
}
```

```go
func rightSideView(root *TreeNode) []int {
	var res []int
	if root == nil {
		return res
	}
	var levels [][]int
	var queue []*TreeNode
	queue = append(queue, root)

	for len(queue) > 0 {
		size, level := len(queue), make([]int, 0)
		for i := 0; i < size; i++ {
			node := queue[0]
			queue = queue[1:]
			level = append(level, node.Val)
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
		}
		levels = append(levels, level)
	}
	for _, level := range levels {
		res = append(res, level[len(level)-1])
	}
	return res
}
```

## [Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/description/)

```java
class Solution {
    /**
     * @param root
     * @return
     */
    public Node connect(Node root) {
        if (root == null) {
            return root;
        }

        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            
            Node prev = null;
            Node node = null;
            for (int i = 0; i < size; i++) {
                if (i == 0) {
                    node = queue.poll();
                    prev = node;
                } else {
                    node = queue.poll();
                    prev.next = node;
                    prev = prev.next;
                }
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            prev.next = null;
        }
        return root;
    }
}
```

```go
func connect(root *Node) *Node {
	if root == nil {
		return root
	}

	var queue []*Node
	queue = append(queue, root)

	for len(queue) > 0 {
		size := len(queue)

		for i := 0; i < size; i++ {
			node := queue[i]
			if i != size-1 {
				queue[i].Next = queue[i+1]
			}
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
		}
		queue = queue[size:]
	}
	return root
}
```

## [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }

        TreeNode node = root.left;
        root.left = root.right;
        root.right = node;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}
```

```go
```

