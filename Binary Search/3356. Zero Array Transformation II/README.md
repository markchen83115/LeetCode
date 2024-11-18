# Leetcode - 3356. Zero Array Transformation II (M)

[Leetcode](https://leetcode.com/problems/zero-array-transformation-ii/)

You are given an integer array `nums` of length `n` and a 2D array `queries` where `queries[i] = [li, ri, vali]`.

Each `queries[i]` represents the following action on `nums`:

-   Decrement the value at each index in the range `[li, ri]` in `nums` by **at most** `vali`.
-   The amount by which each value is decremented can be chosen **independently** for each index.

A **Zero Array** is an array with all its elements equal to 0.

Return the **minimum** possible **non-negative** value of `k`, such that after processing the first `k` queries in **sequence**, `nums` becomes a **Zero Array**. If no such `k` exists, return -1.

**Example 1:**

> **Input:** nums = [2,0,2], queries = [[0,2,1],[0,2,1],[1,1,3]]
> 
> **Output:** 2
> 
> **Explanation:**
> 
> -   **For i = 0 (l = 0, r = 2, val = 1):**
>     -   Decrement values at indices `[0, 1, 2]` by `[1, 0, 1]` respectively.
>     -   The array will become `[1, 0, 1]`.
> -   **For i = 1 (l = 0, r = 2, val = 1):**
>     -   Decrement values at indices `[0, 1, 2]` by `[1, 0, 1]` respectively.
>     -   The array will become `[0, 0, 0]`, which is a Zero Array. Therefore, the minimum value of `k` is 2.

**Example 2:**

> **Input:** nums = [4,3,2,1], queries = [[1,3,2],[0,2,1]]
> 
> **Output:** -1
> 
> **Explanation:**
> 
> -   **For i = 0 (l = 1, r = 3, val = 2):**
>     -   Decrement values at indices `[1, 2, 3]` by `[2, 2, 1]` respectively.
>     -   The array will become `[4, 1, 0, 0]`.
> -   **For i = 1 (l = 0, r = 2, val = 1):**
>     -   Decrement values at indices `[0, 1, 2]` by `[1, 1, 0]` respectively.
>     -   The array will become `[3, 0, 0, 0]`, which is not a Zero Array.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `0 <= nums[i] <= 5 * 105`
-   `1 <= queries.length <= 105`
-   `queries[i].length == 3`
-   `0 <= li <= ri < nums.length`
-   `1 <= vali <= 5`

---
```java
// Java Binary Search 23ms, Time O(NlogN), Space O(N)
class Solution {
    public int minZeroArray(int[] nums, int[][] queries) {
        int n = queries.length;
        int l = 0, r = n + 1;
        while (l < r)
        {
            int mid = l + (r - l) / 2;
            if (isZeroArray(nums, queries, mid))
                r = mid;
            else
                l = mid + 1;
        }

        return l <= n ? l : -1;
    }

    public boolean isZeroArray(int[] nums, int[][] queries, int k) {
        int n = nums.length;
        int[] seg = new int[n + 1];
        for (int i = 0; i < k; i++)
        {
            int[] q = queries[i];
            seg[q[0]] += q[2];
            seg[q[1] + 1] -= q[2];
        }

        int sum = 0;
        for (int i = 0; i < n; i++)
        {
            sum += seg[i];
            if (nums[i] > sum)
                return false;
        }

        return true;
    }
}
```
```java
// Java Greedy 3ms, Time O(N), Space O(N)
class Solution {
    public int minZeroArray(int[] nums, int[][] queries) {
        int n = nums.length;
        int nQuery = queries.length;
        int[] count = new int[n + 1];

        int k = 0;  // 個數
        int sum = 0;
        for (int i = 0; i < n; i++)
        {
            while (sum + count[i] < nums[i])
            {
                k++;
                if (k == nQuery + 1)
                    return -1;
                int l = queries[k - 1][0];
                int r = queries[k - 1][1];
                int val = queries[k - 1][2];

                if (r < i)
                    continue;
                count[Math.max(l, i)] += val;
                count[r + 1] -= val;
            }

            sum += count[i];
        }

        return k;
    }
}
```
---

解1:
對於在多個範圍內做操作, 第一個就想到差分數組
再搭配binary search去搜尋答案
若當前解可行, 則再嘗試縮小當前解
若當前解不可行, 則再放大當前解去尋找可行解

解2:
同樣透過差分數組, 但搭配貪婪的想法
逐一考察nums[i], 若當前k個queries無法使nums[i]變成0, 則往後增加更多個queries
並在過程中紀錄差分數組以備後續使用


###### tags: `Leetcode` `Binary Search` `Binary Search by Value`