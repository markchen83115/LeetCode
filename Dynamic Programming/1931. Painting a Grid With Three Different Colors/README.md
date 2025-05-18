# Leetcode - 1931. Painting a Grid With Three Different Colors (M+)

[Leetcode](https://leetcode.com/problems/painting-a-grid-with-three-different-colors/)

You are given two integers `m` and `n`. Consider an `m x n` grid where each cell is initially white. You can paint each cell **red**, **green**, or **blue**. All cells **must** be painted.

Return_ the number of ways to color the grid with **no two adjacent cells having the same color**_. Since the answer can be very large, return it **modulo** `109 + 7`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/06/22/colorthegrid.png)
> 
> **Input:** m = 1, n = 1
> **Output:** 3
> **Explanation:** The three possible colorings are shown in the image above.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/06/22/copy-of-colorthegrid.png)
> 
> **Input:** m = 1, n = 2
> **Output:** 6
> **Explanation:** The six possible colorings are shown in the image above.

**Example 3:**

> **Input:** m = 5, n = 5
> **Output:** 580986

**Constraints:**

-   `1 <= m <= 5`
-   `1 <= n <= 1000`

---
```java
class Solution {
    public int colorTheGrid(int m, int n) {
        // find valid state
        List<Integer> cand = new ArrayList<>();
        for (int state = 0; state < Math.pow(3, m); state++)
        {
            int s = state;
            int[] tmp = new int[m];
            boolean isOK = true;
            for (int i = 0; i < m; i++)
            {
                int color = s % 3;
                if (i >= 1 && tmp[i - 1] == color)
                {
                    isOK = false;
                    break;
                }
                tmp[i] = color;
                s /= 3;
            }

            if (isOK)
                cand.add(state);
        }

        // dp
        int mod = (int) 1e9 + 7;
        int k = cand.size();
        int[] dp = new int[k];
        Arrays.fill(dp, 1);
        for (int i = 1; i < n; i++)
        {
            int[] dp_new = new int[k];
            for (int s = 0; s < k; s++)
            {
                for (int t = 0; t < k; t++)
                    if (checkOK(cand.get(s), cand.get(t), m))
                        dp_new[s] = (dp_new[s] + dp[t]) % mod;
            }
            
            dp = dp_new;
        }

        // accumulate
        int ret = 0;
        for (int i = 0; i < k; i++)
            ret = (ret + dp[i]) % mod;
        return ret;
    }

    boolean checkOK(int a, int b, int m)
    {
        for (int i = 0; i < m; i++)
        {
            if (a % 3 == b % 3)
                return false;
            a /= 3;
            b /= 3;
        }
        return true;
    }
}
```
---

參考 [【每日一题】1931. Painting a Grid With Three Different Colors - YouTube](https://www.youtube.com/watch?v=YNOy0CCIDjE)

觀察到行數m不超過5，我們就可以考慮以每一列整體作為狀態進行動態規劃。
每一列的狀態數是3^m不超過243，其中符合條件（相鄰不同色）的狀態會更少，m等於5時也只有48

我們可以用一個不大的三進制整數來表示每個格子的噴塗選擇
例如三進位01210就表示一列裡五個格子的顏色

我們考慮第i列的某個狀態s時，唯一的限制因素就是第i-1的狀態
我們遍歷第i-1列的狀態的所有可能t，注意查看是否s和t是否符合並存的條件（即相鄰不同色），然後累加`dp[s]+=dp[t]`

極限資料情況下的時間複雜度大概是`48*48*1000=2e6`，時間複雜度可以接受



###### tags: `Leetcode` `Dynamic Programming` `狀態壓縮DP`