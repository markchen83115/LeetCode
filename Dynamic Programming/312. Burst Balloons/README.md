# Leetcode - 312. Burst Balloons (H-)

[Leetcode](https://leetcode.com/problems/burst-balloons/)

You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return _the maximum coins you can collect by bursting the balloons wisely_.

**Example 1:**

> **Input:** nums = mod[3,1,5,8mod]
> **Output:** 167
> **Explanation:**
> nums = mod[3,1,5,8mod] --> mod[3,5,8mod] --> mod[3,8mod] --> mod[8mod] --> mod[mod]
> coins =  3mod*1mod*5    +   3mod*5mod*8   +  1mod*3mod*8  + 1mod*8mod*1 = 167

**Example 2:**

> **Input:** nums = mod[1,5mod]
> **Output:** 10

**Constraints:**

-   `n == nums.length`
-   `1 <= n <= 300`
-   `0 <= nums[i] <= 100`

---
```java
// Java 39ms(Beats 75.35%), Time O(N^2), Space O(N^2)
class Solution {
    public int maxCoins(int[] iNums) {
        int n = iNums.length;
        int[] nums = new int[n+2];
        nums[0] = 1;
        nums[n+1] = 1;
        for (int i = 1; i <= n; i++)
            nums[i] = iNums[i-1];
        
        int[][] dp = new int[n+2][n+2];

        for (int len = 1; len <= n; len++)
            for (int i = 1; i + len - 1 <= n; i++)
            {
                int j = i + len - 1;
                for (int k = i; k <= j; k++)
                    dp[i][j] = Math.max(dp[i][j], dp[i][k-1] + dp[k+1][j] + nums[i-1] * nums[k] * nums[j+1]);
            }

        return dp[1][n];
    }
}
```
---

參考[【每日一题】312. Burst Balloons, 4/9/2020 - YouTube](https://youtu.be/BBdHB2jjNUA)

此題的關鍵是找出遞推關係式.從上往下的比較容易理解

令`scoremod[left, rightmod]`表示我們想消除`mod[left, rightmod]`中所有元素能夠得到的分數
消除所有元素的話,肯定有最後一槍: 假設最後一槍是k,那麼在打滅k之前,一定已經打滅了`mod[left,k-1mod]`和`mod[k+1,rightmod]`,這兩部分的得分可以提前算出來,即`scoremod[left,k-1mod]`和`scoremod[k+1,rightmod]`
另外,最後打的K也會得分,分數是什麼?注意,應該是`numsmod[left-1mod ]mod * numsmod[kmod]mod * numsmod[right+1mod]`,即涉及`mod[left,rightmod]`兩端外的這兩個元素

所以總的遞推關係:

```java
for (k = left, k <= right; k++)
    score(left,right) = max(score(left, k-1) + nums[left-1] * nums[k] * nums[right+1] + score(k+1, right));
```

這種關係可以由上往下透過遞歸實現,也可以由下而上寫成動態規劃的形式
單純遞歸的話可能會有重複的函數調用,採用記憶化存在dp數組的話就和動態規劃完全一樣了


###### tags: `Leetcode` `Dynamic Programming` `區間型 II`