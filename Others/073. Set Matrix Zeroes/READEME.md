# Leetcode - 73. Set Matrix Zeroes (M)

[Leetcode](https://leetcode.com/problems/set-matrix-zeroes/)

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![](https://miro.medium.com/max/1282/0*NXNpKKEa6gJWgm6O.jpg)
```
Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]  
Output: [[1,0,1],[0,0,0],[1,0,1]]
```
**Example 2:**

![](https://miro.medium.com/max/1400/0*qWqJP4y0_11s-u0w.jpg)
```
Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]  
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```
**Constraints:**

-   `m == matrix.length`
-   `n == matrix[0].length`
-   `1 <= m, n <= 200`
-   `-231 <= matrix[i][j] <= 231 - 1`

**Follow up:**

-   A straightforward solution using `O(mn)` space is probably a bad idea.
-   A simple improvement uses `O(m + n)` space, but still not the best solution.
-   Could you devise a constant space solution?

---

```java
// Java  
class Solution {  
    public void setZeroes(int[][] matrix) {  
        // check if first column / row has 0  
        boolean isColZero = false, isRowZero = false;  
        int m = matrix.length, n = matrix[0].length;  
        for (int i = 0; i < m; i++) {  
            if (matrix[i][0] == 0) {  
                isColZero = true;  
                break;  
            }  
        }  
        for (int j = 0; j < n; j++) {  
            if (matrix[0][j] == 0) {  
                isRowZero = true;  
                break;  
            }  
        }  
  
        // if matrix[i][j] == 0, set matrix[i][0] = 0 and matrix[0][j] = 0  
        for (int i = 1; i < m; i++) {  
            for (int j = 1; j < n; j++) {  
                if (matrix[i][j] == 0) {  
                    matrix[i][0] = 0;  
                    matrix[0][j] = 0;  
                }  
            }  
        }  
  
        // if matrix[i][0] / matrix[0][j]= 0;, set whole column / row to 0  
        for (int i = 1; i < m; i++) {  
            for (int j = 1; j < n; j++) {  
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {  
                    matrix[i][j] = 0;  
                }  
            }  
        }  
  
        // update first column / row  
        if (isColZero) {  
            for (int i = 0; i < m; i++) {  
                matrix[i][0] = 0;  
            }  
        }  
        if (isRowZero) {  
            for (int j = 0; j < n; j++) {  
                matrix[0][j] = 0;  
            }  
        }  
    }  
}
```

---

> **Use first 0th row and 0th column to record 0**

if matrix[i][j] == 0, then set matrix[i][0] = 0 and matrix[0][j] = 0,  
update every column and row to 0 where it’s first column/row is 0


###### tags: `Leetcode`