---
title: Day 14
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 14 Binary tree
data: 2024-11-14
---

# Day 14

## [Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/description/)

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        return countLeftLeavesSum(root, false);
    }
    public int countLeftLeavesSum(TreeNode node, boolean isLeft) {
        if (node == null) {
            return 0;
        }

        if (node.left == null && node.right == null && isLeft) {
            return node.val;
        }

        int sum = 0;
        sum += countLeftLeavesSum(node.left, true);
        sum += countLeftLeavesSum(node.right, false);
        return sum;
    }
}
```

```go
func sumOfLeftLeaves(root *TreeNode) int {
	var countLeftLeavesSum func(node *TreeNode, isLeft bool) int
	countLeftLeavesSum = func(node *TreeNode, isLeft bool) int {
		if node == nil {
			return 0
		}

		if node.Left == nil && node.Right == nil && isLeft {
			return node.Val
		}

		sum := 0
		sum += countLeftLeavesSum(node.Left, true)
		sum += countLeftLeavesSum(node.Right, false)
		return sum
	}

	return countLeftLeavesSum(root, false)
}
```

## [Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/description/)

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        int res = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (i == 0) {
                    res = node.val;
                }

                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return res;
    }
}
```

```go
func findBottomLeftValue(root *TreeNode) int {
	res := 0
	var queue []*TreeNode
	queue = append(queue, root)

	for len(queue) > 0 {
		size := len(queue)
		for i := 0; i < size; i++ {
			node := queue[0]
			queue = queue[1:]
			if i == 0 {
				res = node.Val
			}
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
		}
	}
	return res
}
```

## [Path Sum](https://leetcode.com/problems/path-sum/description/)

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) {
            return false;
        }
        
        if (root.left == null && root.right == null) {
            return targetSum == root.val;
        }

        targetSum -= root.val;
        boolean left = hasPathSum(root.left, targetSum);
        boolean right = hasPathSum(root.right, targetSum);
        return left || right;
    }
}
```

```go
func hasPathSum(root *TreeNode, targetSum int) bool {
	if root == nil {
		return false
	}

	if root.Left == nil && root.Right == nil {
		return targetSum == root.Val
	}

	targetSum -= root.Val
	left := hasPathSum(root.Left, targetSum)
	right := hasPathSum(root.Right, targetSum)
	return left || right
}
```

## [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for (int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }

        return findNode(inorder, 0, inorder.length, postorder, 0, postorder.length);
    }

    public TreeNode findNode(int[] inorder, int inBegin, int inEnd, int[] postorder, int postBegin, int postEnd) {
        if (inBegin >= inEnd && postBegin >= postEnd ) {
            return null;
        }

        int rootIndex = map.get(postorder[postEnd - 1]);
        TreeNode root = new TreeNode(inorder[rootIndex]);
        int leftLength = rootIndex - inBegin;
        root.left = findNode(inorder, inBegin, rootIndex, postorder, postBegin, postBegin + leftLength);
        root.right = findNode(inorder, rootIndex + 1, inEnd, postorder , postBegin + leftLength, postEnd - 1);
        return root;
    }
}
```

```go
var hashmap map[int]int

func buildTree(inorder []int, postorder []int) *TreeNode {
	hashmap = make(map[int]int)
	for i, v := range inorder {
		hashmap[v] = i
	}

	root := findNode(inorder, postorder, len(postorder)-1, 0, len(inorder)-1)
	return root
}

func findNode(inorder []int, postorder []int, rootIdx int, l, r int) *TreeNode {
	if l > r {
		return nil
	}
	if l == r {
		return &TreeNode{Val: inorder[l]}
	}

	rootV := postorder[rootIdx]
	rootIn := hashmap[rootV]
	root := &TreeNode{Val: rootV}
	root.Left = findNode(inorder, postorder, rootIdx-(r-rootIn)-1, l, rootIn-1)
	root.Right = findNode(inorder, postorder, rootIdx-1, rootIn+1, r)
	return root
}

```

