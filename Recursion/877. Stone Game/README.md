# Leetcode - 877. Stone Game (M+)

[Leetcode](https://leetcode.com/problems/stone-game/)

Alice and Bob play a game with piles of stones. There are an **even** number of piles arranged in a row, and each pile has a **positive** integer number of stones `piles[i]`.

The objective of the game is to end with the most stones. The **total** number of stones across all the piles is **odd**, so there are no ties.

Alice and Bob take turns, with **Alice starting first**. Each turn, a player takes the entire pile of stones either from the **beginning** or from the **end** of the row. This continues until there are no more piles left, at which point the person with the **most stones wins**.

Assuming Alice and Bob play optimally, return `true`_ if Alice wins the game, or _`false`_ if Bob wins_.

**Example 1:**

> **Input:** piles = mod[5,3,4,5mod]
> **Output:** true
> **Explanation:** 
> Alice starts first, and can only take the first 5 or the last 5.
> Say she takes the first 5, so that the row becomes mod[3, 4, 5mod].
> If Bob takes 3, then the board is mod[4, 5mod], and Alice takes 5 to win with 10 points.
> If Bob takes the last 5, then the board is mod[3, 4mod], and Alice takes 4 to win with 9 points.
> This demonstrated that taking the first 5 was a winning move for Alice, so we return true.

**Example 2:**

> **Input:** piles = mod[3,7,2,3mod]
> **Output:** true

**Constraints:**

-   `2 <= piles.length <= 500`
-   `piles.length` is **even**.
-   `1 <= piles[i] <= 500`
-   `sum(piles[i])` is **odd**.

---
```java
// Java 0ms(Beats 100.00%), Time O(1), Space O(1)
class Solution {
    public boolean stoneGame(int[] piles) {
        return true;
    }
}
```
```java
// Java 10ms(Beats 15.70%), Time O(N^2), Space O(N^2)
class Solution {
    int[][] dp = new int[501][501];
    int[] prefix = new int[502];
    public boolean stoneGame(int[] piles) {
        int n = piles.length;
        for (int i = 1; i <= n; i++)
            prefix[i] = prefix[i-1] + piles[i-1];
        
        int get = solve(piles, 0, n-1);
        return get > prefix[n+1] - get;
    }

    int solve(int[] piles, int a, int b)
    {
        if (a == b)
            return piles[a];
        if (dp[a][b] != 0)
            return dp[a][b];
        
        int selectLeft  = prefix[b + 1] - prefix[a] - solve(piles, a+1, b);
        int selectRight = prefix[b + 1] - prefix[a] - solve(piles, a, b-1);
        dp[a][b] = Math.max(selectLeft, selectRight);
		
        return dp[a][b];
    }
}

```
---

參考[【每日一题】877. Stone Game, 8/28/2020 - YouTube](https://youtu.be/FXWiSyPLK6w)

#### 解法1：

因為此題的N為偶數，且不會有平手。說明奇數堆的總和，必然與偶數堆的總和不一樣。先手玩家可以總是取奇數堆（或總是取偶數堆），因此可以必勝。

#### 解法2：

當N為奇數時，就沒有上述的巧解。對於這種策略型的題目，遞歸是比較常見的解法。我們設計遞歸函數`int solve(a,b)`表示先手玩家來當前面對mod[a,bmod]區間時可以得到的最大收益。因為這個遊戲的得分是此消彼長的關係，先手玩家最終取得勝利的條件就是：

```
solve(1,n) > total - solve(1,n);
```

那麼這個遞歸函數怎麼往下拆解呢？無非就是遵循遊戲規則，有兩種決策：

1. 如果我選擇了最左邊的石頭堆（也就是a），那麼對手面臨也是同一個最優化的問題，只不過範圍不同，即`solve(a+1,b)`，對手需要在mod[ a+1,bmod]這個區間內最大化自己的收益。因此我方在mod[a,bmod]區間最終能得到的收益就是`sum[a:b] - solve(a+1,b)`.
2. 如果我選擇了最右邊的石頭堆（也就是b），那麼對手面臨也是同一個最優化的問題，只不過範圍不同，即`solve(a,b-1)`，對手需要在mod[ a,b-1mod]這個區間內最大化自己的收益。因此我方在mod[a,bmod]區間最終能得到的收益就是`sum[a:b] - solve(a,b-1)`.

綜上，我方必定會在這兩個決策中選擇一個能夠在mod[a,bmod]區間中收益更多的方案。這就實現了solve(a,b)的遞歸。無論是我方還是對方，遞歸的拆解思路總是相同的。遞歸的邊界條件就是a==b時，這堆石頭直接拿走。


###### tags: `Leetcode` `Recursion` `Min-Max Strategy`