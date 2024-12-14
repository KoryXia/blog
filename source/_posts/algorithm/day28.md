---
title: Day 28
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 28 Greedy
data: 2024-12-13
---

# Day 28

## [Monotone Increasing Digits](https://leetcode.com/problems/monotone-increasing-digits/description/)

```java
class Solution {
    public int monotoneIncreasingDigits(int n) {
        String s = String.valueOf(n);
        char[] chars = s.toCharArray();
        int start = s.length();
        for (int i = s.length() - 2; i >= 0; i--) {
            if (chars[i] > chars[i+1]) {
                chars[i]--;
                start = i + 1;
            }
        }

        for (int i = start; i < s.length(); i++) {
            chars[i] = '9';
        }
        return Integer.parseInt(String.valueOf(chars));
    }
}
```

```go
func monotoneIncreasingDigits(n int) int {
	s := strconv.Itoa(n)
	ss := []byte(s)
	if len(ss) <= 1 {
		return n
	}

	for i := len(ss) - 1; i > 0; i-- {
		if ss[i-1] > ss[i] {
			ss[i-1]--
			for j := i; j < len(ss); j++ {
				ss[j] = '9'
			}
		}
	}
	res, _ := strconv.Atoi(string(ss))
	return res
}
```

## [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/description/)

```java
class Solution {
    int res = 0;
    public int minCameraCover(TreeNode root) {
        if (minCamra(root) == 0) {
            res++;
        }
        return res;
    }

    public int minCamra(TreeNode node) {
        if (node == null) {
            return 2;
        }

        int left = minCamra(node.left);
        int right = minCamra(node.right);

        if (left == 2 && right == 2) {
            return 0;
        } 
        if (left == 0 || right == 0) {
            res++;
            return 1;
        } 
        if (left == 1 || right == 1) {
            return 2;
        }
        return -1;
    }
}
```

```go
func minCameraCover(root *TreeNode) int {
	res := 0
	var minCamera func(node *TreeNode) int
	minCamera = func(node *TreeNode) int {
		if node == nil {
			return 2
		}
		left, right := minCamera(node.Left), minCamera(node.Right)
		if left == 2 && right == 2 {
			return 0
		}

		if left == 0 || right == 0 {
			res++
			return 1
		}

		if left == 1 || right == 1 {
			return 2
		}
		return -1
	}
	if minCamera(root) == 0 {
		res++
	}
	return res
}
```

