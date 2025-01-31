# Leetcode - 827. Making A Large Island (M+)

[Leetcode](https://leetcode.com/problems/making-a-large-island/)

You are given an `n x n` binary matrix `grid`. You are allowed to change **at most one** `0` to be `1`.

Return _the size of the largest **island** in_ `grid` _after applying this operation_.

An **island** is a 4-directionally connected group of `1`s.

**Example 1:**

> **Input:** grid = [[1,0],[0,1]]
> **Output:** 3
> **Explanation:** Change one 0 to 1 and connect two 1s, then we get an island with area = 3.

**Example 2:**

> **Input:** grid = [[1,1],[1,0]]
> **Output:** 4
> **Explanation:** Change the 0 to 1 and make the island bigger, only one island with area = 4.

**Example 3:**

> **Input:** grid = [[1,1],[1,1]]
> **Output:** 4
> **Explanation:** Can't change any 0 to 1, only one island with area = 4.

**Constraints:**

-   `n == grid.length`
-   `n == grid[i].length`
-   `1 <= n <= 500`
-   `grid[i][j]` is either `0` or `1`.

---
```java
// Java 87ms(Beats 74.93%), Time O(M*N), Space O(M*N)
class Solution {
    int[] dir = new int[] {1, 0, -1, 0, 1};
    HashMap<Integer, Integer> islandMap = new HashMap<>();
    public int largestIsland(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int ret = 0;
        int id = 2;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (grid[i][j] == 1)
                {
                    int count = dfs(grid, i, j, id);
                    islandMap.put(id, count);
                    id++;
                }
        
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (grid[i][j] == 0)
                {
                    int count = 1;
                    HashSet<Integer> seenIsland = new HashSet<>();
                    for (int k = 0; k < 4; k++)
                    {
                        int x = i + dir[k];
                        int y = j + dir[k + 1];
                        if (x < 0 || y < 0 || x >= m || y >= n || !islandMap.containsKey(grid[x][y]) || seenIsland.contains(grid[x][y]))
                            continue;
                        seenIsland.add(grid[x][y]);
                        count += islandMap.get(grid[x][y]);
                    }
                    ret = Math.max(ret, count);
                }
        
        if (ret == 0)
            return m * n;

        return ret;
    }

    int dfs(int[][] grid, int i, int j, int id)
    {
        int m = grid.length, n = grid[0].length;
        int ret = 1;

        grid[i][j] = id;        
        for (int k = 0; k < 4; k++)
        {
            int x = i + dir[k];
            int y = j + dir[k + 1];
            if (x < 0 || y < 0 || x >= m || y >= n || grid[x][y] != 1)
                continue;
            ret += dfs(grid, x, y, id);
        }
        
        return ret;
    }
}
```
---

![image](https://hackmd.io/_uploads/r1xClk5_kg.png)
將各個小島面積用`key`存起來

後續尋找`grid[i][j] == 0`的格子, 將其上下左右四格的小島面積相加
需要注意上下左右可能會遇到相同的小島`key`

考慮邊際條件
如果`grid`全部都是`0`, 則`答案 = 1`
如果`grid`全部都是`1`, 則`答案 = M * N`


###### tags: `Leetcode` `DFS`