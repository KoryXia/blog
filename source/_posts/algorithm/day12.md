---
title: Day 12
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 12 Binary tree
---

# Day 12

## [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)

```java
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
func invertTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	node := root.Left
	root.Left = root.Right
	root.Right = node
	invertTree(root.Left)
	invertTree(root.Right)
	return root
}
```

## [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return compare(root.left, root.right);
    }

    public boolean compare(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        } 
        if (left != null && right == null) {
            return false;
        }
        if (left == null && right != null) {
            return false;
        }

        if (left.val != right.val) {
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
		}

		if left != nil && right == nil {
			return false
		}

		if left == nil && right != nil {
			return false
		}

		if left.Val != right.Val {
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
	return max(maxDepth(root.Left), maxDepth(root.Right)) + 1
}
```

## [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

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

	left := minDepth(root.Left)
	right := minDepth(root.Right)

	if left == 0 || right == 0 {
		return left + right + 1
	}

	return min(left, right) + 1
}
```

