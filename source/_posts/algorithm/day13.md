---
title: Day 13
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 13 Binary tree
data: 2024-11-13
---

# Day 13

## [Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/description/)

```java
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int res = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            while (size > 0) {
                TreeNode node = queue.poll();
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
                res++;
                size--;
            }
        }
        return res;        
    }
}
```

```go
func countNodes(root *TreeNode) int {
	if root == nil {
		return 0
	}

	res := 0
	var queue []*TreeNode
	queue = append(queue, root)

	for len(queue) > 0 {
		size := len(queue)
		for i := 0; i < size; i++ {
			node := queue[0]
			queue = queue[1:]
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
			res++
			size--
		}
	}
	return res
}
```

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
        if (left == -1) {
            return -1;
        }
        int right = getHeight(node.right);
        if (right == -1) {
            return -1;
        }

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
			return -1
		}

		left := getHeight(node.Left)
		if left == -1 {
			return -1
		}

		right := getHeight(node.Right)
		if right == -1 {
			return -1
		}

		if left - right > 1 || right - left > 1 {
			return -1
		} else {
			return max(left, right) + 1
		}
	}
	return getHeight(root) != -1
}
```

## [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)

```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        traversal(root, "");
        return res;
    }

    public void traversal(TreeNode node, String path) {
        if (node == null) {
            return;
        }
        if (node.left == null && node.right == null) {
            res.add(new StringBuilder(path).append(node.val).toString());
            return;
        }

        String newPath = new StringBuilder(path).append(node.val).append("->").toString(); 
        traversal(node.left, newPath);
        traversal(node.right, newPath);
    }
}
```

```go
func binaryTreePaths(root *TreeNode) []string {
	res := make([]string, 0)
	var traversal func(node *TreeNode, path string)
	traversal = func(node *TreeNode, path string) {
		if node == nil {
			return
		}
		if node.Left == nil && node.Right == nil {
			res = append(res, path+strconv.Itoa(node.Val))
			return
		}
		path = path + strconv.Itoa(node.Val) + "->"
		traversal(node.Left, path)
		traversal(node.Right, path)
	}
	traversal(root, "")
	return res
}
```

