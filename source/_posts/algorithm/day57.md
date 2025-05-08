---
title: Day 57
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 57 Binary tree
data: 2025-05-08
---

# Day 57

## [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return compare(root.left, root.right);
    }

    public boolean compare(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        } else if (left == null && right != null) {
            return false;
        } else if (left != null && right == null) {
            return false;
        } else if (left.val != right.val) {
            return false;
        }

        boolean outside = compare(left.left, right.right);
        boolean inside = compare(left.right, right.left);
        return outside && inside;
    }
}
```

```go
func isSymmetric(root *TreeNode) bool {
	var compare func(left, right *TreeNode) bool
	compare = func(left, right *TreeNode) bool {
		if left == nil && right == nil {
			return true
		} else if left == nil && right != nil {
			return false
		} else if left != nil && right == nil {
			return false
		} else if left.Val != right.Val {
			return false
		}
		outside := compare(left.Left, right.Right)
		inside := compare(left.Right, right.Left)
		return outside && inside
	}
	return compare(root.Left, root.Right)
}
```

## [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        return Math.max(left, right) + 1;
    }
}
```

```go
func maxDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}
	left, right := maxDepth(root.Left), maxDepth(root.Right)
	return max(left, right) + 1
}
```

## [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

Pay attention to when a node doesn't have a subtree.

```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int left = minDepth(root.left);
        int right = minDepth(root.right);

        if (left == 0 || right == 0) {
            return left + right + 1;
        }
        return Math.min(left, right) + 1;
     }
}
```

```go
func minDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}
	left, right := minDepth(root.Left), minDepth(root.Right)
	if left == 0 || right == 0 {
		return left + right + 1
	}
	return min(left, right) + 1
}
```

## [Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/description/)

```java
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = countNodes(root.left);
        int right = countNodes(root.right);
        return left + right + 1;
    }
}
```

```go
func countNodes(root *TreeNode) int {
	if root == nil {
		return 0
	}
	left, right := countNodes(root.Left), countNodes(root.Right)
	return left + right + 1
}
```

```go
func countNodes(root *TreeNode) int {
	if root == nil {
		return 0
	}
	left, right := root.Left, root.Right
	leftDeepth, rightDeepth := 0, 0
	for left != nil {
		left = left.Left
		leftDeepth++
	}
	for right != nil {
		right = right.Right
		rightDeepth++
	}
	if leftDeepth == rightDeepth { // if it is a full binary tree
		return (2 << leftDeepth) - 1
	}
	return countNodes(root.Left) + countNodes(root.Right) + 1
}
```