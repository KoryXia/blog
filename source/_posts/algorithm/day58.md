---
title: Day 58
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 58 Binary tree
data: 2025-05-09
---

# Day 58

## [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getHeight(root) != -1;
    }

    public int getHeight(TreeNode node) {
        if (node == null) {
            return 0;
        }
        int left = getHeight(node.left);
        if (left == -1) return -1;
        int right = getHeight(node.right);
        if (right == -1) return -1;

        if (Math.abs(left - right) > 1) {
            return -1;
        } else {
            return Math.max(left, right) + 1;
        }
    }
}
```

```go
func isBalanced(root *TreeNode) bool {
	var getHeight func(node *TreeNode) int
	getHeight = func(node *TreeNode) int {
		if node == nil {
			return 0
		}
		left := getHeight(node.Left)
		if left == -1 {
			return -1
		}
		right := getHeight(node.Right)
		if right == -1 {
			return -1
		}

		if left-right > 1 || right-left > 1 {
			return -1
		} else {
			return 1 + max(left, right)
		}
	}
	return getHeight(root) != -1
}
```

## [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        traversal(root, "", res);
        return res;
    }

    public void traversal(TreeNode node, String path, List<String> res) {
        if (node == null) {
            return;
        }

        path += node.val;
        if (node.left == null && node.right == null) {
            res.add(path);
            return;
        }
        traversal(node.left, path + "->", res);
        traversal(node.right, path + "->", res);
    }
}
```

```go
func binaryTreePaths(root *TreeNode) []string {
	res := make([]string, 0)
	var traverse func(node *TreeNode, path string)
	traverse = func(node *TreeNode, path string) {
		if node == nil {
			return
		}

		path += strconv.Itoa(node.Val)
		if node.Left == nil && node.Right == nil {
			res = append(res, path)
			return
		}
		traverse(node.Left, path+"->")
		traverse(node.Right, path+"->")
	}
	traverse(root, "")
	return res
}
```

## [Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/description/)

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int left = sumOfLeftLeaves(root.left);
        int right = sumOfLeftLeaves(root.right);

        int currLeft = 0;
        if (root.left != null && root.left.left == null && root.left.right == null){
            currLeft = root.left.val;
        }

        return left + right + currLeft;
    }
}
```

```go
func sumOfLeftLeaves(root *TreeNode) int {
	if root == nil {
		return 0
	}

	left, right := sumOfLeftLeaves(root.Left), sumOfLeftLeaves(root.Right)
	var currLeft int
	if root.Left != nil && root.Left.Left == nil && root.Left.Right == nil {
		currLeft = root.Left.Val
	}
	return left + right + currLeft
}
```

