# Leetcode - 1072. Flip Columns For Maximum Number of Equal Rows (M)

[Leetcode](https://leetcode.com/problems/flip-columns-for-maximum-number-of-equal-rows/)

You are given an `m x n` binary matrix `matrix`.

You can choose any number of columns in the matrix and flip every cell in that column (i.e., Change the value of the cell from `0` to `1` or vice versa).

Return _the maximum number of rows that have all values equal after some number of flips_.

**Example 1:**

> **Input:** matrix = [[0,1],[1,1]]
> **Output:** 1
> **Explanation:** After flipping no values, 1 row has all values equal.

**Example 2:**

> **Input:** matrix = [[0,1],[1,0]]
> **Output:** 2
> **Explanation:** After flipping values in the first column, both rows have equal values.

**Example 3:**

> **Input:** matrix = [[0,0,0],[0,0,1],[1,1,0]]
> **Output:** 2
> **Explanation:** After flipping values in the first two columns, the last two rows have equal values.

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 300`
-   `matrix[i][j]` is either `0` or `1`.

---
```java
// Java 55ms(Beats 19.10%), Time O(M*N), Space O(N)
class Solution {
    public int maxEqualRowsAfterFlips(int[][] matrix) {
        int ret = 0;
        int m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < m; i++)
        {
            int count = 0;
            int[] flip = new int[n];
            for (int j = 0; j < n; j++)
                flip[j] = 1 - matrix[i][j];
            
            for (int k = 0; k < m; k++)
                if (Arrays.equals(matrix[i], matrix[k]) || Arrays.equals(flip, matrix[k]))
                    count++;

            ret = Math.max(ret, count);
        }

        return ret;
    }
}
```
---

```sql
 [1,0,0,1,0]                                                       [0,0,0,0,0]  // all-0s
 [1,0,0,1,0]  after flipping every cell in 0-th and 3-th columns   [0,0,0,0,0]  // all-0s
 [1,0,1,1,1] ----------------------------------------------------> [0,0,1,0,1]
 [0,1,1,0,1]                                                       [1,1,1,1,1]  // all-1s
 [1,0,0,1,1]                                                       [0,0,0,0,1]
 
 1st, 2nd, and 4th rows have all values equal.
```

想要使得兩個row會相同, 則必須他們原本就相同, 或是他們剛好為完全相反

###### tags: `Leetcode`