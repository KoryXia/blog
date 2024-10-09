---
title: Day 7
tags: Algorithm
category: Algorithm
---

# Day 7

## [Reverse String](https://leetcode.com/problems/reverse-string/description/)

```java
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length - 1;
        while (left < right) {
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;
            left++;
            right--;
        }
    }
}
```

```go
func reverseString(s []byte) {
	for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
		s[i], s[j] = s[j], s[i]
	}
}
```

## [Reverse String II](https://leetcode.com/problems/reverse-string-ii/description/)

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] chars = s.toCharArray();
        for (int i = 0; i < chars.length; i += 2*k) {
            int left = i;
            int right = Math.min(i + k - 1, chars.length - 1);
            while (left < right) {
                char temp = chars[left];
                chars[left] = chars[right];
                chars[right] = temp;
                left++;
                right--;
            }
        }
        return new String(chars);
    }
}
```

```go
func reverseStr(s string, k int) string {
	ss := []byte(s)
	for i := 0; i < len(ss); i += 2 * k {
		left := i
		right := min(left+k-1, len(ss)-1)
		for left < right {
			ss[left], ss[right] = ss[right], ss[left]
			left++
			right--
		}
	}
	return string(ss)
}
```

## Replace numbers

Given a string `s` that contains lowercase alphabetic and numeric characters, write a function that leaves the alphabetic characters in the string unchanged and replaces each numeric character with a `number`.
For example, 

Input:

```
a1b2c3
```

Output:

```
anumberbnumbercnumber
```



```go
package main

import "fmt"

func main() {
	var strByte []byte

	fmt.Scanln(&strByte)

	for i := 0; i < len(strByte); i++ {
		if strByte[i] <= '9' && strByte[i] >= '0' {
			insertElement := []byte{'n', 'u', 'm', 'b', 'e', 'r'}
			strByte = append(strByte[:i], append(insertElement, strByte[i+1:]...)...)
			i = i + len(insertElement) - 1
		}
	}

	fmt.Printf(string(strByte))
}
```

