# Leetcode - 54. Spiral Matrix (M)

[Leetcode](https://leetcode.com/problems/spiral-matrix/)

Given an `m x n` `matrix`, return _all elements of the_ `matrix` _in spiral order_.

**Example 1:**

![](https://miro.medium.com/max/484/0*7wZvAWZv8s6r8S2X.jpg)
```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]  
Output: [1,2,3,6,9,8,7,4,5]
```
**Example 2:**

![](https://miro.medium.com/max/644/0*fuXq1urv3kgoUwhr.jpg)
```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]  
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```
**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 10`
-   `-100 <= matrix[i][j] <= 100`

---

```java
// Java  
// use switch to check every diraction  
class Solution {  
    public List<Integer> spiralOrder(int[][] matrix) {  
        int left = 0, right = matrix[0].length - 1;  
        int up = 0, down = matrix.length - 1;  
        List<Integer> ret = new ArrayList<>();  
        int dir = 0;  
  
        while (left <= right && up <= down) {  
            switch (dir) {  
                // move right  
                case 0:  
                    for (int i = left; i <= right; i++) {  
                        ret.add(matrix[up][i]);  
                    }  
                    up++;  
                    break;  
  
                // move down  
                case 1:  
                    for (int i = up; i <= down; i++) {  
                        ret.add(matrix[i][right]);  
                    }  
                    right--;  
                    break;  
                // move left  
                case 2:  
                    for (int i = right; i >= left; i--) {  
                        ret.add(matrix[down][i]);  
                    }  
                    down--;  
                    break;  
                // move up  
                case 3:  
                    if (left <= right) {  
                        for (int i = down; i >= up; i--) {  
                            ret.add(matrix[i][left]);  
                        }  
                        left++;  
                    }  
                    break;  
            }  
            dir = (dir + 1) % 4;  
        }  
        return ret;  
    }  
}
```
	
```java
// Java  
// without switch, need more if condition  
class Solution {  
    public List<Integer> spiralOrder(int[][] matrix) {  
        int left = 0, right = matrix[0].length - 1;  
        int up = 0, down = matrix.length - 1;  
        List<Integer> ret = new ArrayList<>();  
  
        while (left <= right && up <= down) {  
            // move right  
            for (int i = left; i <= right; i++) {  
                ret.add(matrix[up][i]);  
            }  
            up++;  
            // move down  
            for (int i = up; i <= down; i++) {  
                ret.add(matrix[i][right]);  
            }  
            right--;  
            // move left  
            if (up <= down) {  
                for (int i = right; i >= left; i--) {  
                    ret.add(matrix[down][i]);  
                }  
                down--;  
            }  
            // move up  
            if (left <= right) {  
                for (int i = down; i >= up; i--) {  
                    ret.add(matrix[i][left]);  
                }  
                left++;  
            }  
              
        }  
        return ret;  
    }  
}
```
	
---


從最外圍往右 -> 往下 -> 往左 -> 往上

注意: 往左+往上要檢查該column與該row是否已經被走過  
`if (startRow <= maxRow)`  
`if (startCol <= maxCol)`


###### tags: `Leetcode` `Matrix`