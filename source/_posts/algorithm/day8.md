---
title: Day 8
tags: Algorithm
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 8 String 2
---

# Day 8

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
        while (start <= end) {
            char c = s.charAt(start);
            if (c != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }
            start++;
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

	slow := 0
	for i := 0; i < len(b); i++ {
		if b[i] != ' ' {
			if slow != 0 {
				b[slow] = ' '
				slow++
			}
			for i < len(b) && b[i] != ' ' {
				b[slow] = b[i]
				slow++
				i++
			}
		}
	}
	b = b[0:slow]

	reverse(b)

	last := 0
	for i := 0; i <= len(b); i++ {
		if i == len(b) || b[i] == ' ' {
			reverse(b[last:i])
			last = i + 1
		}
	}
	return string(b)
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

## Right-rotation operation of a string

The right-rotation operation of a string involves shifting a number of characters from the end of the string to the front of the string. Given a string `s` and a positive integer `k`, write a function that implements the right-rotation operation on a string by shifting the trailing `k` characters in the string to the front of the string.
For example, 

Input:

```
s = 'abcdefg', k = 2
```

Output:

```
fgabcde
```

Solutions:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        String s = in.nextLine();

        int len = s.length();
        char[] chars = s.toCharArray();
        reverseString(chars, 0, len - 1);
        reverseString(chars, 0, n - 1);
        reverseString(chars, n, len - 1);

        System.out.println(chars);

    }

    public static void reverseString(char[] ch, int start, int end) {
        while (start < end) {
            ch[start] ^= ch[end];
            ch[end] ^= ch[start];
            ch[start] ^= ch[end];
            start++;
            end--;
        }
    }
}
```

```go
package main

import "fmt"

func reverse(strByte []byte, l, r int) {
	for l < r {
		strByte[l], strByte[r] = strByte[r], strByte[l]
		l++
		r--
	}
}

func main() {
	var str string
	var target int

	fmt.Scanln(&target)
	fmt.Scanln(&str)
	strByte := []byte(str)

	reverse(strByte, 0, len(strByte)-1)
	reverse(strByte, 0, target-1)
	reverse(strByte, target, len(strByte)-1)

	fmt.Printf(string(strByte))
}

```

