---
title: Day 19
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 19 Backtracking
data: 2024-11-25
---

# Day 19

## [Combinations](https://leetcode.com/problems/combinations/description/)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k, 1);
        return res;
    }

    public void backtracking(int n, int k, int start) {
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = start; i <= n - (k - path.size()) + 1; i++) {
            path.add(i);
            backtracking(n, k, i + 1);
            path.removeLast();
        }
    }
}
```

```go
func combine(n int, k int) [][]int {
	path, res := make([]int, 0, k), make([][]int, 0)
	var backtracking func(n, k, start int)
	backtracking = func(n, k, start int) {
		if len(path) == k {
			tmp := make([]int, k)
			copy(tmp, path)
			res = append(res, tmp)
			return
		}

		for i := start; i <= n-(k-len(path))+1; i++ {
			path = append(path, i)
			backtracking(n, k, i+1)
			path = path[:len(path)-1]
		}
	}
	backtracking(n, k, 1)
	return res
}
```

## [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new LinkedList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(k, n, 1, 0);
        return res;
    }

    public void backtracking(int k, int n, int start, int sum) {
        if (sum > n) {
            return;
        }
        if (path.size() > k) {
            return;
        }

        if (sum == n && path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = start; i <= 9; i++) {
            path.add(i);
            backtracking(k, n, i + 1, sum + i);
            path.removeLast();
        }
    }
}
```

```go
func combinationSum3(k int, n int) [][]int {
	path, res := make([]int, 0, k), make([][]int, 0)
	var backtracking func(k, n, sum, start int)
	backtracking = func(k, n, sum, start int) {
		if sum > n {
			return
		}

		if len(path) > k {
			return
		}

		if sum == n && len(path) == k {
			tmp := make([]int, k)
			copy(tmp, path)
			res = append(res, tmp)
			return
		}

		for i := start; i <= 9; i++ {
			path = append(path, i)
			backtracking(k, n, sum+i, i+1)
			path = path[:len(path)-1]
		}
	}
	backtracking(k, n, 0, 1)
	return res
}
```

