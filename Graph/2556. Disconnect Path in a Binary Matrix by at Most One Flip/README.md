# Leetcode - 2556. Disconnect Path in a Binary Matrix by at Most One Flip (H)

[Leetcode](https://leetcode.com/problems/disconnect-path-in-a-binary-matrix-by-at-most-one-flip/description/)

You are given a **0-indexed** `m x n` **binary** matrix `grid`. You can move from a cell `(row, col)` to any of the cells `(row + 1, col)` or `(row, col + 1)` that has the value `1`. The matrix is **disconnected** if there is no path from `(0, 0)` to `(m - 1, n - 1)`.

You can flip the value of **at most one** (possibly none) cell. You **cannot flip** the cells `(0, 0)` and `(m - 1, n - 1)`.

Return `true` _if it is possible to make the matrix disconnect or _`false`_ otherwise_.

**Note** that flipping a cell changes its value from `0` to `1` or from `1` to `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/12/07/yetgrid2drawio.png)

Input: grid = [[1,1,1],[1,0,0],[1,1,1]]
Output: true
Explanation: We can change the cell shown in the diagram above. There is no path from (0, 0) to (2, 2) in the resulting grid.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/12/07/yetgrid3drawio.png)

Input: grid = [[1,1,1],[1,0,1],[1,1,1]]
Output: false
Explanation: It is not possible to change at most one cell such that there is not path from (0, 0) to (2, 2).

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 1000`
-   `1 <= m * n <= 105`
-   `grid[i][j]` is either `0` or `1`.
-   `grid[0][0] == grid[m - 1][n - 1] == 1`

---
```java
// Java 0ms
class Solution {
    int[] dir = new int[] {0, 1, 0, -1, 0};
    public boolean isPossibleToCutPath(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        if (!dfs(grid, 0, 0)) 
            return true;
        grid[0][0] = 1;
        grid[m-1][n-1] = 1;
        return !dfs(grid, 0, 0);
    }
    
    public boolean dfs(int[][] grid, int i, int j) {
        int m = grid.length;
        int n = grid[0].length;
        if (i < 0 || j < 0 || i >= m || j >= n) return false;
        if (grid[i][j] == 0) return false;
        if (i == m-1 && j == n-1) return true;
        grid[i][j] = 0;
        return dfs(grid, i+1, j) || dfs(grid, i, j+1);
    }
}
```

---

先從`[0,0]`走到`[m-1,n-1]`一遍, 走過的點不可再走,
檢查是否可以再從`[0,0]`走到`[m-1,n-1]`一遍,
若可以, 代表有兩條路徑可到終點, 因此不存在flipping point

---

@[saranyamaity2000](https://leetcode.com/saranyamaity2000/)網友指出: 程式正確, 但邏輯錯誤

**Example why any random path removing logic is incorrect**

This Matrix - Rows (N) = 3 , Cols (M) = 4

**1** 1 0 0  
**1 1 1 1**  
1 1 1 **1**  

We will have to reach from (0, 0) to (2, 3)

Assume the following path for this Matrix for DFS1  
(0, 0), **(1, 0), (1, 1), (1, 2), (1, 3)**, (2, 3)

So now you are setting these positions (except (0,0) and (2,3) to Zero according to DFS1 logic.

Now the matrix would be
```
1 1 0 0  
0 0 0 0  
1 1 1 1  
```
**So for DFS2 iteration it will not get any valid path and will return TRUE**  
**but it should return FALSE.**

#### But WHY your implementation is correct ?

**It is because your Code is not VISITING NODES RANDOMLY.**

You are picking like **DOWN DOWN RIGHT** ( if and only if valid DOWN not found) ..... and so on. or (VICE VERSA)

**This BOUNDARY WISE FIRST VALID PATH CHECKING making sure that your will yield correct result.**


###### tags: `Leetcode` `Graph`
