---
title: Day 16
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 16 Binary tree
data: 2024-11-21
---

# Day 16

## [Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)

```java
class Solution {
    TreeNode prev;
    int result = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        if (root == null) {
            return 0;
        }
        traversal(root);
        return result;
    }

    public void traversal(TreeNode node) {
        if (node == null) {
            return;
        }
        traversal(node.left);
        if (prev != null) {
            result = Math.min(result, node.val - prev.val);
        }
        prev = node;
        traversal(node.right);
    }
}
```

```go
func getMinimumDifference(root *TreeNode) int {
	var prev *TreeNode
	result := math.MaxInt64
	var traversal func(node *TreeNode)
	traversal = func(node *TreeNode) {
		if node == nil {
			return
		}
		traversal(node.Left)
		if prev != nil && node.Val-prev.Val < result {
			result = node.Val - prev.Val
		}
		prev = node
		traversal(node.Right)
	}
	traversal(root)
	return result
}
```

## [Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)

```java
class Solution {
    List<Integer> res = new ArrayList<>();
    int maxCount = 0;
    int count;
    TreeNode prev;
    public int[] findMode(TreeNode root) {
        find(root);
        int[] modes = new int[res.size()];
        for (int i = 0; i < res.size(); i++) {
            modes[i] = res.get(i);
        }
        return modes;
    }

    public void find(TreeNode node) {
        if (node == null) {
            return;
        }
        find(node.left);
        if (prev == null || node.val != prev.val) {
            count = 1;
        } else {
            count++;
        }

        if (count > maxCount) {
            res.clear();
            res.add(node.val);
            maxCount = count;
        } else if (count == maxCount) {
            res.add(node.val);
        }

        prev = node;
        find(node.right);
    }
}
```

```go
	func findMode(root *TreeNode) []int {
	res := make([]int, 0)
	count := 1
	maxCount := 1
	var prev *TreeNode
	var traversal func(node *TreeNode)
	traversal = func(node *TreeNode) {
		if node == nil {
			return
		}
		traversal(node.Left)
		if prev == nil || prev.Val != node.Val {
			count = 1
		} else {
			count++
		}

		if count >= maxCount {
			if count != maxCount && len(res) > 0 {
				res = []int{node.Val}
			} else {
				res = append(res, node.Val)
			}
			maxCount = count
		}
		prev = node
		traversal(node.Right)
	}
	traversal(root)
	return res
}
```

## [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || p == root || q == root) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if (left == null && right == null) {
            return null;
        } else if (left == null && right != null) {
            return right;
        } else if (left != null && right == null) {
            return left;
        } else {
            return root;
        }
    }
}
```

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	if root == nil || p == root || q == root {
		return root
	}

	left := lowestCommonAncestor(root.Left, p, q)
	right := lowestCommonAncestor(root.Right, p, q)

	if left == nil && right == nil {
		return nil
	} else if left != nil && right == nil {
		return left
	} else if left == nil && right != nil {
		return right
	} else {
		return root
	}
}
```

