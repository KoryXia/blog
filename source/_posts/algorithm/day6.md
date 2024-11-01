---
title: Day 6
tags: Algorithm
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 6 Hash 2
---

# Day 6

## [4Sum II](https://leetcode.com/problems/4sum-ii/description/)

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int i : nums1) {
            for (int j : nums2) {
                map.put(i + j, map.getOrDefault(i + j, 0) + 1);
            }
        }
        
        int res = 0;
        for (int i : nums3) {
            for (int j : nums4) {
                if (map.containsKey(- (i + j))) {
                    res += map.get(- (i + j));
                }
            }
        }
        return res;
    }
}
```

```go
func fourSumCount(nums1 []int, nums2 []int, nums3 []int, nums4 []int) int {
	m := make(map[int]int)
	res := 0
	for _, v1 := range nums1 {
		for _, v2 := range nums2 {
			m[v1+v2]++
		}
	}

	for _, v3 := range nums3 {
		for _, v4 := range nums4 {
			if count, ok := m[-(v3 + v4)]; ok {
				res += count
			}
		}
	}
	return res
}
```

## [Ransom Note](https://leetcode.com/problems/ransom-note/description/)

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        Map<Character, Integer> map = new HashMap<>();
        for (char c: ransomNote.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        for (char c: magazine.toCharArray()) {
            if (map.containsKey(c)) {
                map.put(c, map.get(c) - 1);
                if (map.get(c) == 0) {
                    map.remove(c);
                }
            }
        }
        return map.size() == 0;
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

## [3Sum](https://leetcode.com/problems/3sum/description/)

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();

        if (nums == null || nums.length < 3) {
            return res;
        }
        
        Arrays.sort(nums);
        
        for (int i = 0; i < nums.length - 2; i++) {
            if (nums[i] > 0) {
                break;
            }

            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    right--;
                    left++;
                } else if (sum > 0) {
                    right--;
                } else {
                    left++;
                }
            }
        }
        return res;
    }
}
```

```go
func threeSum(nums []int) [][]int {
	sort.Ints(nums)
	res := [][]int{}
	for i := 0; i < len(nums)-2; i++ {
		n1 := nums[i]
		if n1 > 0 {
			break
		}
		if i > 0 && n1 == nums[i-1] {
			continue
		}
		l, r := i+1, len(nums)-1
		for l < r {
			n2, n3 := nums[l], nums[r]
			if n1+n2+n3 == 0 {
				res = append(res, []int{n1, n2, n3})
				for l < r && nums[l] == n2 {
					l++
				}
				for l < r && nums[r] == n3 {
					r--
				}
			} else if n1+n2+n3 < 0 {
				l++
			} else {
				r--
			}
		}
	}
	return res
}
```

## [4Sum](https://leetcode.com/problems/4sum/description/)

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;
        for (int a = 0; a < n - 3; a++) { 
            long x = nums[a]; 

            if (a > 0 && x == nums[a - 1]) {
                continue; 
            }
            if (x + nums[a + 1] + nums[a + 2] + nums[a + 3] > target) {
                break;
            }

            if (x + nums[n - 3] + nums[n - 2] + nums[n - 1] < target) {
                continue;
            }
                
            for (int b = a + 1; b < n - 2; b++) {
                long y = nums[b];
                
                if (b > a + 1 && y == nums[b - 1]) {
                    continue;
                }
                if (x + y + nums[b + 1] + nums[b + 2] > target) {
                    break;
                }
                if (x + y + nums[n - 2] + nums[n - 1] < target) {
                    continue;
                }

                int left = b + 1, right = n - 1;

                while (left < right) {
                    long sum = x + y + nums[left] + nums[right];
                    if (sum > target) {
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        ans.add(List.of((int) x, (int) y, nums[left], nums[right]));
                        left++;
                        right--;
                    }
                }
            }
        }
        return ans;
    }
}
```

```go
func fourSum(nums []int, target int) [][]int {
	if len(nums) < 4 {
		return nil
	}
	sort.Ints(nums)
	var res [][]int
	for i := 0; i < len(nums)-3; i++ {
		n1 := nums[i]
		if i > 0 && n1 == nums[i-1] {
			continue
		}
		for j := i + 1; j < len(nums)-2; j++ {
			n2 := nums[j]
			if j > i+1 && n2 == nums[j-1] {
				continue
			}
			l := j + 1
			r := len(nums) - 1
			for l < r {
				n3 := nums[l]
				n4 := nums[r]
				sum := n1 + n2 + n3 + n4
				if sum < target {
					l++
				} else if sum > target {
					r--
				} else {
					res = append(res, []int{n1, n2, n3, n4})
					for l < r && n3 == nums[l+1] {
						l++
					}
					for l < r && n4 == nums[r-1] {
						r--
					}
					r--
					l++
				}
			}
		}
	}
	return res
}
```

