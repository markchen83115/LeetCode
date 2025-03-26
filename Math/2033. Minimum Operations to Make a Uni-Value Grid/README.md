# Leetcode - 2033. Minimum Operations to Make a Uni-Value Grid (M+)

[Leetcode](https://leetcode.com/problems/minimum-operations-to-make-a-uni-value-grid/)

You are given a 2D integer `grid` of size `m x n` and an integer `x`. In one operation, you can **add** `x` to or **subtract** `x` from any element in the `grid`.

A **uni-value grid** is a grid where all the elements of it are equal.

Return _the **minimum** number of operations to make the grid **uni-value**_. If it is not possible, return `-1`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/09/21/gridtxt.png)
> 
> **Input:** grid = [[2,4],[6,8]], x = 2
> **Output:** 4
> **Explanation:** We can make every element equal to 4 by doing the following: 
> - Add x to 2 once.
> - Subtract x from 6 once.
> - Subtract x from 8 twice.
> A total of 4 operations were used.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-1.png)
> 
> **Input:** grid = [[1,5],[2,3]], x = 1
> **Output:** 5
> **Explanation:** We can make every element equal to 3.

**Example 3:**

> ![](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-2.png)
> 
> **Input:** grid = [[1,2],[3,4]], x = 2
> **Output:** -1
> **Explanation:** It is impossible to make every element equal.

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 105`
-   `1 <= m * n <= 105`
-   `1 <= x, grid[i][j] <= 104`

---
```java
// Java 31ms(Beats 90.95%), Time O(MNlogMN), Space O(MN)
class Solution {
    public int minOperations(int[][] grid, int x) {
        int m = grid.length, n = grid[0].length;
        int[] arr = new int[m * n];
        int idx = 0;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
            {
                if (Math.abs(grid[i][j] - grid[0][0]) % x != 0)
                    return -1;
                arr[idx++] = grid[i][j];
            }
        
        Arrays.sort(arr);
        int mid = arr[m * n / 2];
        int ret = 0;
        for (int num : arr)
            ret += Math.abs(num - mid) / x;

        return ret;
    }
}
```
---

參考[【每日一题】LeetCode 2033. Minimum Operations to Make a Uni-Value Grid - YouTube](https://www.youtube.com/watch?v=xYgrb87WWk8)

首先，滿足最終結果的前提條件是，所有矩陣元素與最小值之間的差值必須都是x的倍數。

其次，滿足了上面的條件之後，那麼這個uni-value是什麼呢？
我們希望所有元素變換成uni-value的操作總數最小，也就是希望這個uni-value能和大家都「接近」。

將這個運算總數乘以x後不難發現，也就是希望這個uni-value到所有元素的距離總和最小。
顯然，和`296.Best-Meeting-Point`一樣的數學結論，這個數字一定是所有元素的中位數(median)。

此題的進階版有：`1703.Minimum-Adjacent-Swaps-for-K-Consecutive-Ones`, `1478.Allocate-Mailboxes`



###### tags: `Leetcode` `Math` `Median Theorem`