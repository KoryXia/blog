---
title: Day 15
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 15 Binary tree
---

# Day 15

## [Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/description/)

```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return buildTree(nums, 0, nums.length);
    }

    public TreeNode buildTree(int[] nums, int leftIndex, int rightIndex) {
        if (rightIndex - leftIndex < 1) {
            return null;
        }
        if (rightIndex - leftIndex == 1) {
            return new TreeNode(nums[leftIndex]);
        }
        int maxIndex = leftIndex;
        int maxValue = nums[maxIndex];
        for (int i = leftIndex + 1; i < rightIndex; i++) {
            if (nums[i] > maxValue) {
                maxValue = nums[i];
                maxIndex = i;
            }
        }
        TreeNode root = new TreeNode(maxValue);
        root.left = buildTree(nums, leftIndex, maxIndex);
        root.right = buildTree(nums, maxIndex + 1, rightIndex);
        return root;
    }
}
```

```go
func constructMaximumBinaryTree(nums []int) *TreeNode {
	var buildTree func(nums []int, leftIndex, rightIndex int) *TreeNode
	buildTree = func(nums []int, leftIndex, rightIndex int) *TreeNode {
		if rightIndex-leftIndex < 1 {
			return nil
		}
		if rightIndex-leftIndex == 1 {
			return &TreeNode{Val: nums[leftIndex]}
		}

		maxIndex := leftIndex
		maxValue := nums[leftIndex]
		for i := leftIndex + 1; i < rightIndex; i++ {
			if nums[i] > maxValue {
				maxValue = nums[i]
				maxIndex = i
			}
		}

		root := &TreeNode{Val: maxValue}
		root.Left = buildTree(nums, leftIndex, maxIndex)
		root.Right = buildTree(nums, maxIndex+1, rightIndex)
		return root
	}
	return buildTree(nums, 0, len(nums))
}
```

## [Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/description/)

```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) {
            return root2;
        }
        if (root2 == null) {
            return root1;
        }

        root1.val += root2.val;
        root1.left = mergeTrees(root1.left, root2.left);
        root1.right = mergeTrees(root1.right, root2.right);
        return root1;
    }
}
```

```go
func mergeTrees(root1 *TreeNode, root2 *TreeNode) *TreeNode {
	if root1 == nil {
		return root2
	}
	if root2 == nil {
		return root1
	}

	root1.Val += root2.Val
	root1.Left = mergeTrees(root1.Left, root2.Left)
	root1.Right = mergeTrees(root1.Right, root2.Right)
	return root1
}
```

## [Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) {
            return root;
        }

        if (val < root.val) {
            return searchBST(root.left, val);
        } else {
            return searchBST(root.right, val);
        }
    }
}
```

```go
func searchBST(root *TreeNode, val int) *TreeNode {
	if root == nil || root.Val == val {
		return root
	}

	if val < root.Val {
		return searchBST(root.Left, val)
	} else {
		return searchBST(root.Right, val)
	}
}
```

## [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)

```java
class Solution {
    private long prev = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        boolean left = isValidBST(root.left);
        if (!left) {
            return false;
        }

        if (prev >= root.val) {
            return false;
        }
        prev = root.val;

        boolean right = isValidBST(root.right);
        return right;
    }
}
```

> **Important**
>
>  In Go, all function arguments are passed by value, including when using recursion. This means that when you pass a variable to a function, Go creates a copy of the variables, and any modifications made to that copy will not affect the original variable in the calling function.

```go
func check(root *TreeNode, max *int) bool {
	if root == nil {
		return true
	}

	left := check(root.Left, max)
	if !left {
		return false
	}

	if *max >= root.Val {
		return false
	}
	*max = root.Val

	right := check(root.Right, max)
	return right
}

func isValidBST(root *TreeNode) bool {
	prev := math.MinInt64
	return check(root, &prev)
}
```

