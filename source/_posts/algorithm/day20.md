---
title: Day 20
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 20 Backtracking
data: 2024-11-26
---

# Day 20

## [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

```java
class Solution {
    List<String> res = new ArrayList<>();
    StringBuffer sb = new StringBuffer();
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) {
            return res;
        }
        String[] numString = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        backtracking(0, digits, numString);
        return res;
    }

    public void backtracking(int index, String digits, String[] numString) {
        if (index == digits.length()) {
            res.add(sb.toString());
            return;
        }

        String str = numString[digits.charAt(index) - '0'];
        for (int i = 0; i < str.length(); i++) {
            sb.append(str.charAt(i));
            backtracking(index + 1, digits, numString);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```

```go
func letterCombinations(digits string) []string {
	res, path := make([]string, 0), make([]byte, 0)
	m := []string{"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"}

	if digits == "" {
		return res
	}

	var backtracking func(index int)
	backtracking = func(index int) {
		if len(path) == len(digits) {
			tmp := string(path)
			res = append(res, tmp)
			return
		}
		digit := int(digits[index] - '0')
		str := m[digit-2]
		for i := 0; i < len(str); i++ {
			path = append(path, str[i])
			backtracking(index + 1)
			path = path[:len(path)-1]
		}
	}
	backtracking(0)
	return res
}
```

## [Combination Sum](https://leetcode.com/problems/combination-sum/description/)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new LinkedList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtracking(0, 0, target, candidates);
        return res;
    }

    public void backtracking(int start, int sum, int target, int[] candidates) {
        if (sum > target) {
            return;
        }

        if (sum == target) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = start; i < candidates.length; i++) {
            path.add(candidates[i]);
            backtracking(i, sum + candidates[i], target, candidates);
            path.removeLast();
        }
    }
}
```

```go
func combinationSum(candidates []int, target int) [][]int {
	res, path := make([][]int, 0), make([]int, 0)
	sort.Ints(candidates)
	var backtracking func(sum, start int)
	backtracking = func(sum, start int) {
		if sum > target {
			return
		}

		if sum == target {
			tmp := make([]int, len(path))
			copy(tmp, path)
			res = append(res, tmp)
			return
		}

		for i := start; i < len(candidates); i++ {
			path = append(path, candidates[i])
			backtracking(sum+candidates[i], i)
			path = path[:len(path)-1]
		}
	}
	backtracking(0, 0)
	return res
}
```

## [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new LinkedList<>();

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtracking(0, 0, target, candidates);
        return res;
    }

    public void backtracking(int start, int sum, int target, int[] candidates) {
        if (sum > target) {
            return;
        }

        if (sum == target) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = start; i < candidates.length; i++) {
            if (i > start && candidates[i] == candidates[i - 1]) {
                continue;
            }
            path.add(candidates[i]);
            backtracking(i + 1, sum + candidates[i], target, candidates);
            path.removeLast();
        }
    }
}
```

```go
func combinationSum2(candidates []int, target int) [][]int {
	path, res := make([]int, 0), make([][]int, 0)
	sort.Ints(candidates)
	var backtracking func(sum, start int)
	backtracking = func(sum, start int) {
		if sum > target {
			return
		}
		if sum == target {
			tmp := make([]int, len(path))
			copy(tmp, path)
			res = append(res, tmp)
		}

		for i := start; i < len(candidates); i++ {
			if i > start && candidates[i] == candidates[i-1] {
				continue
			}
			path = append(path, candidates[i])
			backtracking(sum+candidates[i], i+1)
			path = path[:len(path)-1]
		}
	}
	backtracking(0, 0)
	return res
}
```

## [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/description/)

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    List<String> path = new LinkedList<>();

    public List<List<String>> partition(String s) {
        backtracking(s, 0);
        return res;
    }

    public void backtracking(String s, int start) {
        if (start >= s.length()) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = start; i < s.length(); i++) {
            String str = s.substring(start, i + 1);
            if (check(str)){
                path.add(str);
                backtracking(s, i + 1);
                path.removeLast();
            }
        }
    }

    public boolean check(String sb) {
        for (int i = 0; i < sb.length() / 2; i++) {
            if (sb.charAt(i) != sb.charAt(sb.length() - 1 - i)) {
                return false;
            }
        }
        return true;
    }
}
```

```go
func partition(s string) [][]string {
	path, res := make([]string, 0), make([][]string, 0)
	var backtracking func(start int)
	backtracking = func(start int) {
		if start == len(s) {
			tmp := make([]string, len(path))
			copy(tmp, path)
			res = append(res, tmp)
			return
		}

		for i := start; i < len(s); i++ {
			str := s[start : i+1]
			if isPalindrome(str) {
				path = append(path, str)
				backtracking(i + 1)
				path = path[:len(path)-1]
			}
		}
	}
	backtracking(0)
	return res
}
func isPalindrome(s string) bool {
	for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
		if s[i] != s[j] {
			return false
		}
	}
	return true
}
```

