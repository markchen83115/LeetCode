# Leetcode - 3449. Maximize the Minimum Game Score (H-)

[Leetcode](https://leetcode.com/problems/maximize-the-minimum-game-score/)

You are given an array `points` of size `n` and an integer `m`. There is another array `gameScore` of size `n`, where `gameScore[i]` represents the score achieved at the `ith` game. Initially, `gameScore[i] == 0` for all `i`.

You start at index -1, which is outside the array (before the first position at index 0). You can make **at most** `m` moves. In each move, you can either:

-   Increase the index by 1 and add `points[i]` to `gameScore[i]`.
-   Decrease the index by 1 and add `points[i]` to `gameScore[i]`.

**Note** that the index must always remain within the bounds of the array after the first move.

Return the **maximum possible minimum** value in `gameScore` after **at most** `m` moves.

**Example 1:**

> **Input:** points = [2,4], m = 3
> 
> **Output:** 4
> 
> **Explanation:**
> 
> Initially, index `i = -1` and `gameScore = [0, 0]`.
> 
> | Move | Index | gameScore |
> | --- | --- | --- |
> | Increase `i` | 0 | `[2, 0]` |
> | Increase `i` | 1 | `[2, 4]` |
> | Decrease `i` | 0 | `[4, 4]` |
> 
> The minimum value in `gameScore` is 4, and this is the maximum possible minimum among all configurations. Hence, 4 is the output.

**Example 2:**

> **Input:** points = [1,2,3], m = 5
> 
> **Output:** 2
> 
> **Explanation:**
> 
> Initially, index `i = -1` and `gameScore = [0, 0, 0]`.
> 
> | Move | Index | gameScore |
> | --- | --- | --- |
> | Increase `i` | 0 | `[1, 0, 0]` |
> | Increase `i` | 1 | `[1, 2, 0]` |
> | Decrease `i` | 0 | `[2, 2, 0]` |
> | Increase `i` | 1 | `[2, 4, 0]` |
> | Increase `i` | 2 | `[2, 4, 3]` |
> 
> The minimum value in `gameScore` is 2, and this is the maximum possible minimum among all configurations. Hence, 2 is the output.

**Constraints:**

-   `2 <= n == points.length <= 5 * 104`
-   `1 <= points[i] <= 106`
-   `1 <= m <= 109`

---
```java
// Java 167ms(Beats 100.00%), Time O(N*log(1e15)), Space O(1)
class Solution {
    public long maxScore(int[] points, int m) {
        long ret = 0;
        long l = 0, r = (long) 1e15;
        while (l < r)
        {
            long mid = r - (r - l) / 2;
            if (isOK(points, m, mid))
                l = mid;
            else
                r = mid - 1;
        }
        return l;
    }

    boolean isOK(int[] points, int m, long x)
    {
        int n = points.length;
        long count = 1;
        long cur = points[0];
        for (int i = 0; i < n; i++)
        {
            if (i == n - 1) // last one
            {
                if (cur >= x)
                    return true;
                
                long d = (x - cur - 1) / points[i] + 1;
                return count + 2 * d <= m;
            }

            if (cur >= x)
            {
                count++;
                if (count > m)
                    return false;
                cur = points[i + 1];
            }
            else
            {
                long d = (x - cur - 1) / points[i] + 1;

                // if n - 2 done and also n - 1 done, no need to go n - 1, like Example 1
                if (i == n - 2 && points[i + 1] * d >= x && count + 2 * d <= m)
                    return true;

                count += 2 * d + 1;
                if (count > m)
                    return false;
                cur = points[i + 1] * (d + 1);
            }
        }
        return true;
    }
}
```
---

參考[【每日一题】LeetCode 3449. Maximize the Minimum Game Score - YouTube](https://youtu.be/N6aScon-ehY)

考慮到`m`的範圍異常的大，本題極有可能是二分搜值
我們猜測一個值`X`，然後檢驗是否能在`m`次移動後，使得所有的元素都大於等於`X`

移動的策略似乎很明顯可以貪心。當我在0號位置的時候，如果還沒有實現得分大於`X`，必然會通過先朝右再朝左的反覆橫跳`d`次，直至滿足0號位置大於等於`X`

為什麼只選擇在0號和1號位置的反覆橫跳而不是更大的幅度？感覺沒有必要
如果更大幅度的重複橫跳，不僅在0號位置和1號位置上各自增加`d`次賦分，而且會在2號及之後的位置上也增加`d`次賦分，但這些賦分是否值得呢？不見得

因此，我們只需要老實每次做幅度為1的來回橫跳即可

綜上，我們的演算法是：當我們來到`i`時，查看在該位置是否已經超過了預期得分
如果沒有，那就計算還需要幾次賦分（假設記作`d`次）。然後再做`=>(i+1)=>i`的`d`次重複移動
在`i`位置上滿足之後，再移動依序到`i+1`的位置上，此時注意我們已經在`i+1`的位置上得到了`points[i+1]*(d+1)`的分數。然後重複上述的過程

需要特別注意的邊界邏輯有兩個地方：

1. 如果走到了最後一個位置，仍沒有超過預期得分，那麼只能進行「往左再往右」的反覆橫跳
2. 如果走到了倒數第二個位置，經過幾次橫跳之後，發現在此位置和下一個位置都已經滿足了得分預期，那麼最後一步可以不用再走了


###### tags: `Leetcode` `Binary Search` `Binary Search by Value`