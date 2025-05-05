---
title: Day 53
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 53 String
data: 2025-05-03
---

# Day 53

Learn KMP!!!

## [Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int[] next = getPrefix(needle);
        int j = 0;
        for (int i = 0; i < haystack.length(); i++) {
            while (j > 0 && haystack.charAt(i) != needle.charAt(j)) {
                j = next[j - 1];
            }
            if (haystack.charAt(i) == needle.charAt(j)) {
                j++;
            }
            if (j == needle.length()) {
                return i - j + 1;
            }
        }
        return -1;
    }

    public int[] getPrefix(String pattern) {
        int[] prefix = new int[pattern.length()];
        prefix[0] = 0;
        int j = 0;
        for (int i = 1; i < prefix.length; i++) {
            while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
                j = prefix[j - 1];
            }
            if (pattern.charAt(i) == pattern.charAt(j)) {
                j++;
            }
            prefix[i] = j;
        }
        return prefix;
    }
}
```

```go
func strStr(haystack string, needle string) int {
	next, j := make([]int, len(needle)), 0
	getPrefix(next, needle)
	for i := 0; i < len(haystack); i++ {
		for j > 0 && haystack[i] != needle[j] {
			j = next[j-1]
		}
		if haystack[i] == needle[j] {
			j++
		}
		if j == len(needle) {
			return i - j + 1
		}
	}
	return -1
}
func getPrefix(next []int, pattern string) {
	j := 0
	next[0] = 0
	for i := 1; i < len(pattern); i++ {
		for j > 0 && pattern[i] != pattern[j] {
			j = next[j-1]
		}
		if pattern[i] == pattern[j] {
			j++
		}
		next[i] = j
	}
}
```

## [Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/description/)

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        // return (s+s).indexOf(s, 1) != s.length();
        int len = s.length();
        int[] next = new int[len];
        next[0] = 0;
        int j = 0;
        for (int i = 1; i < len; i++) {
            while (j > 0 && s.charAt(i) != s.charAt(j)) {
                j = next[j - 1];
            }
            if (s.charAt(i) == s.charAt(j)) {
                j++;
            }
            next[i] = j;
        }
        return next[len - 1] > 0 && len % (len - next[len - 1]) == 0;
    }
}
```

```go
func repeatedSubstringPattern(s string) bool {
	n := len(s)
	next, j := make([]int, n), 0
	next[0] = 0
	for i := 1; i < n; i++ {
		for j > 0 && s[i] != s[j] {
			j = next[j-1]
		}
		if s[i] == s[j] {
			j++
		}
		next[i] = j
	}

	return next[n-1] > 0 && n%(n-next[n-1]) == 0
}
```

