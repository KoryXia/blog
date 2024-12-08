---
title: Day 21
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 21 Backtracking
data: 2024-11-27
---

# Day 21

## [Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)

```java
class Solution {
    List<String> res = new ArrayList<>();

    public List<String> restoreIpAddresses(String s) {
        StringBuilder sb = new StringBuilder(s);
        backtracking(sb, 0, 0);
        return  res;

    }

    public void backtracking(StringBuilder sb, int startIndex, int count) {
        if (count == 3 && check(sb, startIndex, sb.length() - 1)) {
            res.add(sb.toString());
            return;
        }

        for (int i = startIndex; i <= startIndex + 3 && i < sb.length(); i++) {
            if (check(sb, startIndex, i)) {
                sb.insert(i + 1, ".");
                backtracking(sb, i + 2, count + 1);
                sb.deleteCharAt(i + 1);
            } else {
                break;
            }
        }
    }

    public boolean check(StringBuilder s, int start, int end) {
        if (start > end) {
            return false;
        }

        if (s.charAt(start) == '0' && start != end) {
            return false;
        }

        int num = 0;
        for (int i = start; i <= end; i++) {
            if (s.charAt(end)> '9' || s.charAt(i) < '0') {
                return false;
            }
            num = num * 10 + (s.charAt(i) - '0');
            if (num > 255) {
                return false;
            }
        }
        return true;
    }
}
```

```go
func restoreIpAddresses(s string) []string {
	path, res := make([]string, 0), make([]string, 0)
	var backtracking func(start int)
	backtracking = func(start int) {
		if len(path) == 4 && start == len(s) {
			str := strings.Join(path, ".")
			res = append(res, str)
			return
		}

		for i := start; i <= start+3 && i < len(s); i++ {
			if i != start && s[start] == '0' {
				break
			}

			str := s[start : i+1]
			num, _ := strconv.Atoi(str)
			if num >= 0 && num < 256 {
				path = append(path, str)
				backtracking(i + 1)
				path = path[:len(path)-1]
			} else {
				break
			}
		}
	}
	backtracking(0)
	return res
}
```

## [Subsets](https://leetcode.com/problems/subsets/description/)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new LinkedList<>();
    
    public List<List<Integer>> subsets(int[] nums) {
        backtracking(0, nums);
        return res;
    }

    public void backtracking(int start, int[] nums) {
        res.add(new ArrayList<>(path));

        if (start >= nums.length) {
            return;
        }

        for (int i = start; i < nums.length; i++) {
            path.add(nums[i]);
            backtracking(i + 1, nums);
            path.removeLast();
        }
    }
}
```

```go
func subsets(nums []int) [][]int {
	res, path := make([][]int, 0), make([]int, 0)

	var backtracking func(start int)
	backtracking = func(start int) {
		tmp := make([]int, len(path))
		copy(tmp, path)
		res = append(res, tmp)
		if start >= len(nums) {
			return
		}

		for i := start; i < len(nums); i++ {
			path = append(path, nums[i])
			backtracking(i + 1)
			path = path[:len(path)-1]
		}
	}
	backtracking(0)
	return res
}
```

## [Subsets II](https://leetcode.com/problems/subsets-ii/description/)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new LinkedList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        backtracking(0, nums);
        return res;
    }

    public void backtracking(int start, int[] nums) {
        res.add(new ArrayList<>(path));

        if (start >= nums.length) {
            return;
        }

        for (int i = start; i < nums.length; i++) {
            if (i > start && nums[i - 1] == nums[i]) {
                continue;
            }
            path.add(nums[i]);
            backtracking(i + 1, nums);
            path.removeLast();
        }
    }
}
```

```go
func subsetsWithDup(nums []int) [][]int {
	sort.Ints(nums)
	res, path := make([][]int, 0), make([]int, 0)

	var backtracking func(start int)
	backtracking = func(start int) {
		tmp := make([]int, len(path))
		copy(tmp, path)
		res = append(res, tmp)
		if start >= len(nums) {
			return
		}

		for i := start; i < len(nums); i++ {
			if i > start && nums[i-1] == nums[i] {
				continue
			}
			path = append(path, nums[i])
			backtracking(i + 1)
			path = path[:len(path)-1]
		}
	}
	backtracking(0)
	return res
}
```