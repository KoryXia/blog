---
title: Day 49
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 49 HashMap
data: 2025-04-26
---

# Day 49

## [Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()) {
            return false;
        }
        Map<Character, Integer> map = new HashMap<>();

        for (char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }

        for (char c : t.toCharArray()) {
            if (!map.containsKey(c)) {
                return false;
            }
            int count = map.get(c);
            if (count == 0) {
                return false;
            }
            map.put(c, count - 1);
        }
        return true;
    }
}
```

```go
func isAnagram(s string, t string) bool {
	if len(s) != len(t) {
		return false
	}

	byteMap := make(map[byte]int, len(s))

	for i := 0; i < len(s); i++ {
		byteMap[s[i]]++
		byteMap[t[i]]--
	}

	for _, v := range byteMap {
		if v != 0 {
			return false
		}
	}
	return true
}
```

## [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/description/)

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if (nums1.length == 0 || nums2.length == 0) {
            return new int[0];
        }
        Set<Integer> set = new HashSet<>();
        for (int num: nums1) {
            set.add(num);
        }

        Set<Integer> res = new HashSet<>();
        for (int num: nums2) {
            if (set.contains(num)) {
                res.add(num);
            }
        }
        return res.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

```go
func intersection(nums1 []int, nums2 []int) []int {
	set := make(map[int]bool)
	for _, v := range nums1 {
		set[v] = true
	}

	var res []int

	for _, v := range nums2 {
		if set[v] {
			res = append(res, v)
			delete(set, v)
		}
	}

	return res
}
```

## [Happy Number](https://leetcode.com/problems/happy-number/description/)

```java
class Solution {
    public int getNext(int n) {
        int sum = 0;
        while(n > 0) {
            int temp = n % 10;
            sum += temp * temp;
            n = n / 10;
        }
        return sum;
    }

    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();
        while (n != 1 && !set.contains(n)) {
            set.add(n);
            n = getNext(n);
        }
        return n == 1;
    }
}
```

```go
func isHappy(n int) bool {
	m := make(map[int]bool)
	for n != 1 && !m[n] {
		m[n] = true
		n = next(n)
	}
	return n == 1
}

func next(n int) int {
	sum := 0
	for n > 0 {
		sum += (n % 10) * (n % 10)
		n = n / 10
	}
	return sum
}
```

