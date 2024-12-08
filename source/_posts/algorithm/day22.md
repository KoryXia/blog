---
title: Day 22
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 22 Backtracking
data: 2024-12-02
---

# Day 22

## [Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/description/)

```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> path = new LinkedList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        backTracking(nums, 0);
        return result;
    }
    private void backTracking(int[] nums, int startIndex){
        if(path.size() >= 2) {
            result.add(new ArrayList<>(path));            
        }
        HashSet<Integer> hs = new HashSet<>();
        for(int i = startIndex; i < nums.length; i++){
            if(!path.isEmpty() && path.get(path.size() -1 ) > nums[i] || hs.contains(nums[i])) {
                continue;
            }
            hs.add(nums[i]);
            path.add(nums[i]);
            backTracking(nums, i + 1);
            path.removeLast();
        }
    }
}
```

```go
func findSubsequences(nums []int) [][]int {
	res, path := make([][]int, 0), make([]int, 0)
	var backtracking func(start int)
	backtracking = func(start int) {
		if len(path) >= 2 {
			tmp := make([]int, len(path))
			copy(tmp, path)
			res = append(res, tmp)
		}

		hs := make(map[int]bool, len(nums))
		for i := start; i < len(nums); i++ {
			if len(path) != 0 && path[len(path)-1] > nums[i] || hs[nums[i]] {
				continue
			}

			hs[nums[i]] = true
			path = append(path, nums[i])
			backtracking(i + 1)
			path = path[:len(path)-1]
		}
	}
	backtracking(0)
	return res
}
```

## [Permutations](https://leetcode.com/problems/permutations/description/)

```java
class Solution {
    private List<List<Integer>> res = new ArrayList<>();
    private List<Integer> path = new LinkedList<>();
    public List<List<Integer>> permute(int[] nums) {
        backtracking(nums, new boolean[nums.length]);
        return res;
    }

    public void backtracking(int[] nums, boolean[] used) {
        if (path.size() == nums.length) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (used[i]) {
                continue;
            }
            used[i] = true;
            path.add(nums[i]);
            backtracking(nums, used);
            path.removeLast();
            used[i] = false;
        }
    }
}
```

```go
func permute(nums []int) [][]int {
	path, res := make([]int, 0), make([][]int, 0)
	used := make([]bool, len(nums))
	var backtracking func()
	backtracking = func() {
		if len(path) == len(nums) {
			tmp := make([]int, len(path))
			copy(tmp, path)
			res = append(res, tmp)
			return
		}

		for i := 0; i < len(nums); i++ {
			if used[i] {
				continue
			}

			used[i] = true
			path = append(path, nums[i])
			backtracking()
			path = path[:len(path)-1]
			used[i] = false
		}
	}
	backtracking()
	return res
}
```

## [Permutations II](https://leetcode.com/problems/permutations-ii/description/)

```java
class Solution {
    private List<List<Integer>> res = new ArrayList<>();
    private List<Integer> path = new LinkedList<>();
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        backtracking(nums, new boolean[nums.length]);
        return res;
    }

    public void backtracking(int[] nums, boolean[] used) {
        if (path.size() == nums.length) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (i > 0 && nums[i - 1] == nums[i] && !used[i - 1]) {
                continue;
            }
            if (!used[i]) {
                used[i] = true;
                path.add(nums[i]);
                backtracking(nums, used);
                path.removeLast();
                used[i] = false;
            }
        }
    }
}
```

```go
func permuteUnique(nums []int) [][]int {
	path, res := make([]int, 0), make([][]int, 0)
	used := make([]bool, len(nums))
	var backtracking func()
	backtracking = func() {
		if len(path) == len(nums) {
			tmp := make([]int, len(path))
			copy(tmp, path)
			res = append(res, tmp)
			return
		}

		for i := 0; i < len(nums); i++ {
			if i > 0 && nums[i-1] == nums[i] && !used[i-1] {
				continue
			}
			if !used[i] {
				used[i] = true
				path = append(path, nums[i])
				backtracking()
				path = path[:len(path)-1]
				used[i] = false
			}
		}
	}
	backtracking()
	return res
}
```

