# Leetcode - 2661. First Completely Painted Row or Column (M-)

[Leetcode](https://leetcode.com/problems/first-completely-painted-row-or-column/)

You are given a **0-indexed** integer array `arr`, and an `m x n` integer **matrix** `mat`. `arr` and `mat` both contain **all** the integers in the range `[1, m * n]`.

Go through each index `i` in `arr` starting from index `0` and paint the cell in `mat` containing the integer `arr[i]`.

Return _the smallest index_ `i` _at which either a row or a column will be completely painted in_ `mat`.

**Example 1:**

> ![image explanation for example 1](https://assets.leetcode.com/uploads/2023/01/18/grid1.jpg)
> 
> **Input:** arr = [1,3,4,2], mat = [[1,4],[2,3]]
> **Output:** 2
> **Explanation:** The moves are shown in order, and both the first row and second column of the matrix become fully painted at arr[2].

**Example 2:**

> ![image explanation for example 2](https://assets.leetcode.com/uploads/2023/01/18/grid2.jpg)
> 
> **Input:** arr = [2,8,7,4,1,3,5,6,9], mat = [[3,2,5],[1,4,6],[8,7,9]]
> **Output:** 3
> **Explanation:** The second column becomes fully painted at arr[3].

**Constraints:**

-   `m == mat.length`
-   `n = mat[i].length`
-   `arr.length == m * n`
-   `1 <= m, n <= 105`
-   `1 <= m * n <= 105`
-   `1 <= arr[i], mat[r][c] <= m * n`
-   All the integers of `arr` are **unique**.
-   All the integers of `mat` are **unique**.

---
```java
// Java 14ms(Beats 86.07%), Time O(M*N), Space O(M*N)
class Solution {
    public int firstCompleteIndex(int[] arr, int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int[] row = new int[m];
        int[] col = new int[n];
        Arrays.fill(row, n);
        Arrays.fill(col, m);
        int[][] nums = new int[m * n + 1][2];
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                nums[mat[i][j]] = new int[] {i, j};

        for (int i = 0; i < arr.length; i++)
        {
            int k = arr[i];
            int r = nums[k][0];
            int c = nums[k][1];
            if (--row[r] == 0 || --col[c] == 0)
                return i;
        }

        return arr.length - 1;
    }
}
```
---

將`arr[i]`對應到的row與col分別減去1,
直到有一個`row = 0 || col = 0`, 代表該row或column已塗滿

注意: 
總共`m`個row, 每個row會有`n`個數字
總共`n`個col, 每個col會有`m`個數字


###### tags: `Leetcode` `Graph`