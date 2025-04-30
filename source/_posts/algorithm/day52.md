---
title: Day 52
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 52 String
data: 2025-04-30
---

# Day 52

## [Reverse String](https://leetcode.com/problems/reverse-string/description/)

```java
class Solution {
    public void reverseString(char[] s) {
        int l = 0;
        int r = s.length - 1;
        while (l < r) {
            char temp = s[l];
            s[l] = s[r];
            s[r] = temp;
            l++;
            r--;
        }
    }
}
```

```go
func reverseString(s []byte) {
	for l, r := 0, len(s)-1; l < r; l, r = l+1, r-1 {
		s[l], s[r] = s[r], s[l]
	}
}
```

## [Reverse String II](https://leetcode.com/problems/reverse-string-ii/description/)

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] sc = s.toCharArray();
        for (int i = 0; i < sc.length; i += 2 * k) {
            int left = i;
            int right = Math.min(i + k - 1, sc.length - 1);
            while (left < right) {
                char temp = sc[left];
                sc[left] = sc[right];
                sc[right] = temp;
                left++;
                right--;
            }
        }
        return new String(sc);
    }
}
```

```go
func reverseStr(s string, k int) string {
	bs := []byte(s)
	for i := 0; i < len(bs); i += 2 * k {
		left, right := i, min(i+k-1, len(bs)-1)
		for left < right {
			bs[left], bs[right] = bs[right], bs[left]
			left++
			right--
		}
	}
	return string(bs)
}
```

## [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/description/)

```java
class Solution {
    public String reverseWords(String s) {
        StringBuilder sb = removeSpace(s);
        reverseStr(sb, 0, sb.length() - 1);
        reverseEachWord(sb);
        return sb.toString();
    }

    private StringBuilder removeSpace(String s) {
        int start = 0;
        int end = s.length() - 1;
        while (s.charAt(start) == ' ') start++;
        while (s.charAt(end) == ' ') end--;
        StringBuilder sb = new StringBuilder();
        for (int i = start; i <= end; i++) {
            if (s.charAt(i) != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                sb.append(s.charAt(i));
            }
        }
        return sb;
    }

    public void reverseStr(StringBuilder sb, int start, int end) {
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
        }
    }


    private void reverseEachWord(StringBuilder sb) {
        int start = 0;
        int end = 1;
        int n = sb.length();
        while (start < n) {
            while (end < n && sb.charAt(end) != ' ') {
                end++;
            }
            reverseStr(sb, start, end - 1);
            start = end + 1;
            end = start + 1;
        }
    }
}
```

```go
func reverseWords(s string) string {
	b := []byte(s)
	var res []byte
	l, r := 0, len(b)-1
	for b[l] == ' ' {
		l++
	}
	for b[r] == ' ' {
		r--
	}
	for i := l; i <= r; i++ {
		if b[i] != ' ' || res[len(res)-1] != ' ' {
			res = append(res, b[i])
		}
	}

	reverse(res)

	l, r = 0, 1

	for l < len(res) {
		for r < len(res) && res[r] != ' ' {
			r++
		}
		reverse(res[l:r])
		l = r + 1
		r = l + 1
	}
	return string(res)
}

func reverse(b []byte) {
	left := 0
	right := len(b) - 1
	for left < right {
		b[left], b[right] = b[right], b[left]
		left++
		right--
	}
}
```

