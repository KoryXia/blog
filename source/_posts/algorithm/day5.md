---
title: Day 5
tags: Algorithm
category: Algorithm
---

# Day 5

## [Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        
        Map<Character, Integer> map = new HashMap<>();
        
        for (char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
            
        }

        for (char c : t.toCharArray()) {
            if (map.get(c) == null || map.get(c) == 0) {
                return false;
            }
            
            map.put(c, map.get(c) - 1);
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
        Set<Integer> set = new HashSet<>();
        for (int num : nums1) {
            set.add(num);
        }

        Set<Integer> res = new HashSet<>();

        for (int num : nums2) {
            if (set.contains(num)) {
                res.add(num);
            }
        }
        
        return res.stream().mapToInt(v -> v).toArray();
    }
}
```

```go
func intersection(nums1 []int, nums2 []int) []int {
	set := make(map[int]bool)
	res := make([]int, 0)
	for _, v := range nums1 {
		if _, ok := set[v]; !ok {
			set[v] = true
		}
	}

	for _, v := range nums2 {
		if _, ok := set[v]; ok {
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
    public int next(int i) {
        int sum = 0;
        while (i > 0) {
            int d = i % 10;
            sum += d * d;
            i /= 10;
        }
        return sum;
    }

    public boolean isHappy(int n) {
        int slow = n;
        int fast = next(n);
        while (fast != 1 && slow != fast){
            slow = next(slow);
            fast = next(next(fast));
        }
        return fast == 1;
    }
}
```

```go
func isHappy(n int) bool {
	slow := n
	fast := next(n)
	for fast != 1 && slow != fast {
		slow = next(slow)
		fast = next(next(fast))
	}
	return fast == 1
}

func next(n int) int {
	sum := 0

	for n > 0 {
		sum += (n % 10) * (n % 10)
		n /= 10
	}
	return sum
}
```

## [Two Sum](https://leetcode.com/problems/two-sum/description/)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int temp = target - nums[i];
            if (map.containsKey(temp)) {
                return new int[] {i, map.get(temp)};
            }
            map.put(nums[i], i);
        }
        return null;
    }
}
```

```go
func twoSum(nums []int, target int) []int {
	numsMap := make(map[int]int)

	for i, num := range nums {
		complement := target - num

		if index, ok := numsMap[complement]; ok {
			return []int{index, i}
		}

		numsMap[num] = i
	}
	return nil
}
```

