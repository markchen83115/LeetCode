# Leetcode - 1975. Maximum Matrix Sum (M)

[Leetcode](https://leetcode.com/problems/maximum-matrix-sum/)

You are given an `n x n` integer `matrix`. You can do the following operation **any** number of times:

-   Choose any two **adjacent** elements of `matrix` and **multiply** each of them by `-1`.

Two elements are considered **adjacent** if and only if they share a **border**.

Your goal is to **maximize** the summation of the matrix's elements. Return _the **maximum** sum of the matrix's elements using the operation mentioned above._

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/07/16/pc79-q2ex1.png)
> 
> **Input:** matrix = [[1,-1],[-1,1]]
> **Output:** 4
> **Explanation:** We can follow the following steps to reach sum equals 4:
> - Multiply the 2 elements in the first row by -1.
> - Multiply the 2 elements in the first column by -1.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/07/16/pc79-q2ex2.png)
> 
> **Input:** matrix = [[1,2,3],[-1,-2,-3],[1,2,3]]
> **Output:** 16
> **Explanation:** We can follow the following step to reach sum equals 16:
> - Multiply the 2 last elements in the second row by -1.

**Constraints:**

-   `n == matrix.length == matrix[i].length`
-   `2 <= n <= 250`
-   `-105 <= matrix[i][j] <= 105`

---
```java
// Java 7ms(Beats 29.87%), Time O(N^2), Space O(1)
class Solution {
    public long maxMatrixSum(int[][] matrix) {
        int n = matrix.length;
        int count = 0;
        long ret = 0;
        long min = 100005;
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
            {
                ret += Math.abs(matrix[i][j]);
                min = Math.min(min, Math.abs(matrix[i][j]));
                if (matrix[i][j] < 0)
                    count++;
            }
        
        count %= 2;
        if (count == 0)
            return ret;
        
        return ret - 2 * min;
    }
}
```
---

在相鄰的格子中都乘上-1, 代表能任意上下左右移動負號
只要將兩個負號移動到隔壁, 便可消除他們
因此偶數個負號必可全消除

###### tags: `Leetcode` `Others`