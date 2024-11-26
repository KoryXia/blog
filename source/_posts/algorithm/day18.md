---
title: Day 18
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 18 Binary tree
data: 2024-11-24
---

# Day 18

## [Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/description/)

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) {
            return null;
        }

        if (root.val < low) {
            return trimBST(root.right, low, high);
        }

        if (root.val > high) {
            return trimBST(root.left, low, high);
        } 

        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        return root;
    }
}
```

```go
func trimBST(root *TreeNode, low int, high int) *TreeNode {
	if root == nil {
		return nil
	}

	if root.Val < low {
		return trimBST(root.Right, low, high)
	}

	if root.Val > high {
		return trimBST(root.Left, low, high)
	}

	root.Left = trimBST(root.Left, low, high)
	root.Right = trimBST(root.Right, low, high)
	return root
}
```

## [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return build(nums, 0, nums.length);
    }

    public TreeNode build(int[] nums, int left, int right) {
        if (left >= right) {
            return null;
        }

        if (right - left == 1) {
            return new TreeNode(nums[left]);
        }

        int mid = left + (right - left) / 2;
        TreeNode node = new TreeNode(nums[mid]);
        node.left = build(nums, left, mid);
        node.right = build(nums, mid + 1, right);
        return node;
    }
}
```

```go
func sortedArrayToBST(nums []int) *TreeNode {
	var build func(nums []int, left, right int) *TreeNode

	build = func(nums []int, left, right int) *TreeNode {
		if left >= right {
			return nil
		}

		if right-left == 1 {
			return &TreeNode{Val: nums[left]}
		}

		mid := left + (right-left)/2
		node := &TreeNode{Val: nums[mid]}
		node.Left = build(nums, left, mid)
		node.Right = build(nums, mid+1, right)
		return node
	}
	return build(nums, 0, len(nums))
}
```

## [Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)

```java
class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        traversal(root);
        return root;
    }

    public void traversal(TreeNode node) {
        if (node == null) {
            return;
        }

        traversal(node.right);
        sum += node.val;
        node.val = sum;
        traversal(node.left);
    }
}
```

```go
func convertBST(root *TreeNode) *TreeNode {
	sum := 0
	var traversal func(node *TreeNode)
	traversal = func(node *TreeNode) {
		if node == nil {
			return
		}
		traversal(node.Right)
		sum += node.Val
		node.Val = sum
		traversal(node.Left)
	}
	traversal(root)
	return root
}
```

