# Leetcode - 1277. Count Square Submatrices with All Ones (M)

[Leetcode](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

Given a `m * n` matrix of ones and zeros, return how many **square** submatrices have all ones.

**Example 1:**

> **Input:** matrix =
> [
>   [0,1,1,1],
>   [1,1,1,1],
>   [0,1,1,1]
> ]
> **Output:** 15
> **Explanation:** 
> There are **10** squares of side 1.
> There are **4** squares of side 2.
> There is  **1** square of side 3.
> Total number of squares = 10 + 4 + 1 = **15**.

**Example 2:**

> **Input:** matrix = 
> [
>   [1,0,1],
>   [1,1,0],
>   [1,1,0]
> ]
> **Output:** 7
> **Explanation:** 
> There are **6** squares of side 1.  
> There is **1** square of side 2. 
> Total number of squares = 6 + 1 = **7**.

**Constraints:**

-   `1 <= arr.length <= 300`
-   `1 <= arr[0].length <= 300`
-   `0 <= arr[i][j] <= 1`

---
```java
// Java 7ms(Beats 45.12%), Time O(M*N), Space O(M*N)
class Solution {
    public int countSquares(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        int ret = 0;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
            {
                if (matrix[i][j] > 0 && i >= 1 && j >= 1)
                {
                    matrix[i][j] = 1 + Math.min(matrix[i - 1][j - 1], Math.min(matrix[i - 1][j], matrix[i][j - 1]));
                }
                ret += matrix[i][j];
            }

        return ret;
    }

}
```
---

解這類題目有一個非常有名的動態轉移方程，就是`dp[i][j] = min(dp[i-1][j-1],dp[i][j-1],dp[i-1][j])+1`

`dp[i][j]`: 以`(i,j)`為右下角的正方形有多大

範例:  
```
1, 1, 1  
1, 1, 1  
1, 1, 1
```
`dp[0][0] = 1`  
`dp[1][1] = 2` (the square including `A[1][1]` can be square with element `A[1][1]` with size: 2 * 2, 1 * 1)  
`dp[2][2] = 3` (the square including `A[2][2]` can be square with element `A[2][2]` with size: 3 * 3, 2 * 2, 1 * 1)


###### tags: `Leetcode` `Dynamic Programming`