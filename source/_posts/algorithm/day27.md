---
title: Day 27
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 27 Greedy
data: 2024-12-12
---

# Day 27

## [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/)

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        
        int res = 1;
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] >= intervals[i - 1][1]) {
                res++;
            } else {
                intervals[i][1] = Math.min(intervals[i - 1][1], intervals[i][1]);
            }
        }
        return intervals.length - res;
    }
}
```

```go
func eraseOverlapIntervals(intervals [][]int) int {
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})

	res := 1

	for i := 1; i < len(intervals); i++ {
		if intervals[i][0] >= intervals[i-1][1] {
			res++
		} else {
			intervals[i][1] = min(intervals[i-1][1], intervals[i][1])
		}
	}
	return len(intervals) - res
}

func min(a, b int) int {
	if a > b {
		return b
	}
	return a
}
```

## [Partition Labels](https://leetcode.com/problems/partition-labels/description/)

```java
class Solution {
    public List<Integer> partitionLabels(String s) {
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            map.put(s.charAt(i), i);
        }

        int start = 0;
        int end = 0;
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < s.length(); i++) {
            end = Math.max(end, map.get(s.charAt(i)));
            if (i == end) {
                res.add(end - start + 1);
                start = end + 1;
            }
        }
        return res;
    }
}
```

```go
func partitionLabels(s string) []int {
	var marks [26]int

	for i := 0; i < len(s); i++ {
		marks[s[i]-'a'] = i
	}

	res := make([]int, 0)
	start, end := 0, 0

	for i := 0; i < len(s); i++ {
		end = max(end, marks[s[i]-'a'])
		if i == end {
			res = append(res, end-start+1)
			start = end + 1
		}
	}
	return res
}

func max(a, b int) int {
	if a < b {
		a = b
	}
	return a
}
```

## [Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        List<int[]> list = new ArrayList<>();
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        int start = intervals[0][0];
        int end = intervals[0][1];

        for (int i = 1; i < intervals.length; i++) {
            int left = intervals[i][0];
            int right = intervals[i][1];

            if (left > end) {
                list.add(new int[] {start, end});
                start = left;
            }

            end = Math.max(end, right);
        }
        
        list.add(new int[] {start, end});
        return list.toArray(new int[list.size()][]);
    }
}
```

```go
func merge(intervals [][]int) [][]int {
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})

	res, start, end := make([][]int, 0), intervals[0][0], intervals[0][1]

	for i := 0; i < len(intervals); i++ {
		left, right := intervals[i][0], intervals[i][1]
		if left > end {
			res = append(res, []int{start, end})
			start = left
		}

		end = max(end, right)
	}
	res = append(res, []int{start, end})
	return res
}

func max(a, b int) int {
	if a < b {
		a = b
	}
	return a
}
```
