---
title: Day 50
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 50 HashMap
data: 2025-04-28
---

# Day 50

## [Two Sum](https://leetcode.com/problems/two-sum/description/)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int temp = target - nums[i];
            if (map.containsKey(temp)) {
                return new int[]{map.get(temp), i};
            }
            map.put(nums[i], i);
        }
        return null;
    }
}
```

```go
func twoSum(nums []int, target int) []int {
	m := make(map[int]int)

	for i, v := range nums {
		temp := target - v
		if _, ok := m[temp]; ok {
			return []int{m[temp], i}
		}
		m[nums[i]] = i
	}
	return nil
}

```

## [4Sum II](https://leetcode.com/problems/4sum-ii/description/)

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();
        int res = 0;
        for (int i = 0; i < nums1.length; i++) {
            for (int j = 0; j < nums2.length; j++) {
                int sum = nums1[i] + nums2[j];
                map.put(sum, map.getOrDefault(sum, 0) + 1);
            }
        }

        for (int i = 0; i < nums3.length; i++) {
            for (int j = 0; j < nums4.length; j++) {
                int sum = -(nums3[i] + nums4[j]);
                if (map.containsKey(sum)) {
                    res += map.get(sum);
                }
            }
        }
        return res;
    }
}
```

```go
func fourSumCount(nums1 []int, nums2 []int, nums3 []int, nums4 []int) int {
	m, res := make(map[int]int), 0

	for _, v1 := range nums1 {
		for _, v2 := range nums2 {
			m[v1+v2]++
		}
	}

	for _, v3 := range nums3 {
		for _, v4 := range nums4 {
			res += m[-(v3 + v4)]
		}
	}
	return res
}
```

## [Ransom Note](https://leetcode.com/problems/ransom-note/description/)

`Magazine` first, then `ransomNote`

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if (ransomNote.length() > magazine.length()) return false;
        int[] record = new int[26];

        for (char c : magazine.toCharArray()) {
            record[c - 'a']++;
        }
        
        for (char c : ransomNote.toCharArray()) {
            record[c - 'a']--;
            if (record[c - 'a'] < 0) {
                return false;
            }
        }
        
        for (int v : record) {
            if (v < 0) {
                return false;
            }
        }
        return true;
    }
}
```

```go
func canConstruct(ransomNote string, magazine string) bool {
	record := make([]int, 26)
	for _, v := range magazine {
		record[v-'a']++
	}

	for _, v := range ransomNote {
		record[v-'a']--
		if record[v-'a'] < 0 {
			return false
		}
	}
	return true
}
```

