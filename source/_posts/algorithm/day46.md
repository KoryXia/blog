---
title: Day 46
tags: [Algorithm]
category: Algorithm
cover: /img/algorithm_cover.png
description: Day 46 Array
data: 2025-04-17
---

# Day 46

## [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/description/)

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        int top = 0;
        int bottom = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;
    

        while (top <= bottom && left <= right)  {
            for (int j = left; j <= right; j++) {
                res.add(matrix[top][j]);
            }
            top++;
    
            for (int i = top; i <= bottom; i++) {
                res.add(matrix[i][right]);
            }
            right--;
            
            if (top <= bottom) {
                for (int j = right; j >= left; j--) {
                    res.add(matrix[bottom][j]);
                }
                bottom--;
            }
            
            if (left <= right) {
                for (int i = bottom; i >= top; i--) {
                    res.add(matrix[i][left]);
                }
                left++;
            }
        }

        return res;
    }
}
```

```go
func spiralOrder(matrix [][]int) []int {
	res := []int{}
	top, bottom, left, right := 0, len(matrix)-1, 0, len(matrix[0])-1
	for top <= bottom && left <= right {
		// right
		for j := left; j <= right; j++ {
			res = append(res, matrix[top][j])
		}
		top++

		// down
		for i := top; i <= bottom; i++ {
			res = append(res, matrix[i][right])
		}
		right--

		// left
		if top <= bottom {
			for j := right; j >= left; j-- {
				res = append(res, matrix[bottom][j])
			}
			bottom--
		}

		// up
		if left <= right {
			for i := bottom; i >= top; i-- {
				res = append(res, matrix[i][left])
			}
			left++
		}
	}
	return res
}
```

## [Length of Last Word](https://leetcode.com/problems/length-of-last-word/description/)

```java
class Solution {
    public int lengthOfLastWord(String s) {
        int count = 0;
        for (int i = s.length() - 1; i >= 0; i--) {
            if (isAlphabet(s.charAt(i))) {
                count++;
            } else if (count > 0) {
                break;
            }
        }
        return count;
    }

    public boolean isAlphabet(char a) {
        if ((a <= 'z' && a >= 'a') || (a <= 'Z' && a >= 'A')) {
            return true;
        }
        return false;
    }
}
```

```go
func lengthOfLastWord(s string) int {
	count := 0
	for i := len(s) - 1; i >= 0; i-- {
		if isAlphabet(s[i]) {
			count++
		} else if count > 0 {
			break
		}
	}
	return count
}

func isAlphabet(c byte) bool {
	return (c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z')
}
```

