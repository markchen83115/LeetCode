# Leetcode - 3219. Minimum Cost for Cutting Cake II (H)

[Leetcode](https://leetcode.com/problems/minimum-cost-for-cutting-cake-ii/)

There is an `m x n` cake that needs to be cut into `1 x 1` pieces.

You are given integers `m`, `n`, and two arrays:

-   `horizontalCut` of size `m - 1`, where `horizontalCut[i]` represents the cost to cut along the horizontal line `i`.
-   `verticalCut` of size `n - 1`, where `verticalCut[j]` represents the cost to cut along the vertical line `j`.

In one operation, you can choose any piece of cake that is not yet a `1 x 1` square and perform one of the following cuts:

1.  Cut along a horizontal line `i` at a cost of `horizontalCut[i]`.
2.  Cut along a vertical line `j` at a cost of `verticalCut[j]`.

After the cut, the piece of cake is divided into two distinct pieces.

The cost of a cut depends only on the initial cost of the line and does not change.

Return the **minimum** total cost to cut the entire cake into `1 x 1` pieces.

> **Example 1:**
> 
> **Input:** m = 3, n = 2, horizontalCut = [1,3], verticalCut = [5]
> 
> **Output:** 13
> 
> **Explanation:**
> 
> ![](https://assets.leetcode.com/uploads/2024/06/04/ezgifcom-animated-gif-maker-1.gif)
> 
> -   Perform a cut on the vertical line 0 with cost 5, current total cost is 5.
> -   Perform a cut on the horizontal line 0 on `3 x 1` subgrid with cost 1.
> -   Perform a cut on the horizontal line 0 on `3 x 1` subgrid with cost 1.
> -   Perform a cut on the horizontal line 1 on `2 x 1` subgrid with cost 3.
> -   Perform a cut on the horizontal line 1 on `2 x 1` subgrid with cost 3.
> 
> The total cost is `5 + 1 + 1 + 3 + 3 = 13`.

**Example 2:**

> **Input:** m = 2, n = 2, horizontalCut = [7], verticalCut = [4]
> 
> **Output:** 15
> 
> **Explanation:**
> 
> -   Perform a cut on the horizontal line 0 with cost 7.
> -   Perform a cut on the vertical line 0 on `1 x 2` subgrid with cost 4.
> -   Perform a cut on the vertical line 0 on `1 x 2` subgrid with cost 4.
> 
> The total cost is `7 + 4 + 4 = 15`.

**Constraints:**

-   `1 <= m, n <= 105`
-   `horizontalCut.length == m - 1`
-   `verticalCut.length == n - 1`
-   `1 <= horizontalCut[i], verticalCut[i] <= 103`


---
```java
// Java 68ms(Beats 84.55%), Time O(NlogN), Space O(1)
class Solution {
    public long minimumCost(int m, int n, int[] horizontalCut, int[] verticalCut) {
        long ret = 0;
        Arrays.sort(horizontalCut);
        Arrays.sort(verticalCut);
        long horSum = 0, verSum = 0;
        for (int h : horizontalCut)
            horSum += h;
        for (int v : verticalCut)
            verSum += v;
        int i = m - 2, j = n - 2;

        while (i >= 0 && j >= 0)
        {
            int h = horizontalCut[i], v = verticalCut[j];
            if (h >= v)
            {
                ret += h + verSum;
                horSum -= h;
                i--;
            }
            else
            {
                ret += v + horSum;
                verSum -= v;
                j--;
            }
        }

        ret += verSum + horSum;
        return ret;
    }
}
```

---
參考: [【每日一题】LeetCode 3219. Minimum Cost for Cutting Cake II - YouTube](https://youtu.be/RsTV4u3hsxg)

從垂直與水平中最大的cost開始切,
因為每切一刀, 會產生多一倍的相對方向數量
ex: 垂直切一刀 -> 產生水平要多切一刀

並且已切過水平, 不會因為垂直切一刀而需要再切一刀, 
因此只有剩餘未切水平會需要多切一刀 (相反垂直也一樣)

所以每切一刀, 需要將該刀cost從未切的cost中移除


###### tags: `Leetcode` `Greedy`