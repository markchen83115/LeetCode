# Leetcode - 3413. Maximum Coins From K Consecutive Bags (H-)

[Leetcode](https://leetcode.com/problems/maximum-coins-from-k-consecutive-bags/)

There are an infinite amount of bags on a number line, one bag for each coordinate. Some of these bags contain coins.

You are given a 2D array `coins`, where `coins[i] = [li, ri, ci]` denotes that every bag from `li` to `ri` contains `ci` coins.

The segments that `coins` contain are non-overlapping.

You are also given an integer `k`.

Return the **maximum** amount of coins you can obtain by collecting `k` consecutive bags.

**Example 1:**

> **Input:** coins = [[8,10,1],[1,3,2],[5,6,4]], k = 4
> 
> **Output:** 10
> 
> **Explanation:**
> 
> Selecting bags at positions `[3, 4, 5, 6]` gives the maximum number of coins: `2 + 0 + 4 + 4 = 10`.

**Example 2:**

> **Input:** coins = [[1,10,3]], k = 2
> 
> **Output:** 6
> 
> **Explanation:**
> 
> Selecting bags at positions `[1, 2]` gives the maximum number of coins: `3 + 3 = 6`.

**Constraints:**

-   `1 <= coins.length <= 105`
-   `1 <= k <= 109`
-   `coins[i] == [li, ri, ci]`
-   `1 <= li <= ri <= 109`
-   `1 <= ci <= 1000`
-   The given segments are non-overlapping.

---
```java
// Java 87ms(Beats 100.00%), Time O(NlogN), Space O(N)
class Solution {
    public long maximumCoins(int[][] coins, int k) {
        long ret = 0;
        int n = coins.length;
        // left to right
        Arrays.sort(coins, (a, b) -> a[0] - b[0]);
        ret = helper(coins, k);

        // right to left
        for (int i = 0; i < n; i++)
        {
            int a = coins[i][0], b = coins[i][1];
            coins[i][0] = -b;
            coins[i][1] = -a;
        }
        Arrays.sort(coins, (a, b) -> a[0] - b[0]);
        ret = Math.max(ret, helper(coins, k));

        return ret;
    }

    long helper(int[][] coins, int k)
    {
        long ret = 0;
        int j = 0;
        int n = coins.length;
        long sum = 0;
        for (int i = 0; i < n; i++)
        {
            int end = coins[i][0] + k - 1;
            // add j
            while (j < n && coins[j][1] <= end)
            {
                sum += (long)(coins[j][1] - coins[j][0] + 1) * coins[j][2];
                j++;
            }

            long extra = 0;
            if (j < n && coins[j][0] <= end)
                extra += (long)(end - coins[j][0] + 1) * coins[j][2];
            
            ret = Math.max(ret, sum + extra);

            // remove i
            sum -= (long)(coins[i][1] - coins[i][0] + 1) * coins[i][2];
        }

        return ret;
    }
}
```
---

參考[【每日一题】LeetCode 3413. Maximum Coins From K Consecutive Bags - YouTube](https://youtu.be/24G933ceqNM)

此題和`2271.Maximum-White-Tiles-Covered-by-a-Carpet`的思路類似。

對於長度為`k`的跨度，如果其一個端點沒有落在任何區間，那麼顯然是不划算的。我們必然有更優的策略：平移這段跨度直至一端接觸到某個區間的邊緣，這樣可以在另一端覆蓋到更多的有效區域得到更大的價值。注意「某個區間的端點」可以是左端點，也可以是右端點。

再考慮，對於長度為`k`的跨度，如果其兩個端點分別都落在了區間`A`和區間`B`內，那麼同樣也是不划算的。只要區間`A`和`B`的價值密度不一樣，那麼我們必然能找到更優的解，也就是朝價值密度更高的那個方向平移即可。平移的最終結果是：完全離開價值密度低的區間（如果另一端仍在價值密度高的區間的話），或觸碰到價值密度高的區間的邊緣。

所以上述的結論就是，最優解的情況，必然發生在所選跨度恰好觸碰在某個區間邊緣的時候。所以我們分兩種情況。首先，從左往右遍歷每個區間的左邊緣，當做是所選跨度k的左邊界，然後可以確定右邊界的位置，這樣就計算總價值；隨著對左邊界的挨個嘗試，右邊界也是單調移動的。所以這是一個典型的雙指標。然後，反過來，從右往左遍歷每個區間的右邊緣，當做是所選跨度的右邊界，然後可以確定左邊界的位置，這樣就計算總價值；隨著對右邊界的挨個嘗試，左邊界也是單調移動的。

對於第二次遍歷，我們可以重複利用第一次遍歷的函數。只要將每個區間的左右端點完全顛倒即可。即原區間範圍是`[a,b]`，那麼我們建構一個新的區間範圍`[-b,-a]`。這樣我們依然可以重複利用從左往右遍歷的程式碼，本質上實現了從右往左的遍歷。


###### tags: `Leetcode` `Greedy` `Intervals`