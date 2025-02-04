# Leetcode - 1052. Grumpy Bookstore Owner (M)

[Leetcode](https://leetcode.com/problems/grumpy-bookstore-owner/)

There is a bookstore owner that has a store open for `n` minutes. You are given an integer array `customers` of length `n` where `customers[i]` is the number of the customers that enter the store at the start of the `ith` minute and all those customers leave after the end of that minute.

During certain minutes, the bookstore owner is grumpy. You are given a binary array grumpy where `grumpy[i]` is `1` if the bookstore owner is grumpy during the `ith` minute, and is `0` otherwise.

When the bookstore owner is grumpy, the customers entering during that minute are not **satisfied**. Otherwise, they are satisfied.

The bookstore owner knows a secret technique to remain **not grumpy** for `minutes` consecutive minutes, but this technique can only be used **once**.

Return the **maximum** number of customers that can be _satisfied_ throughout the day.

**Example 1:**

> **Input:** customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], minutes = 3
> 
> **Output:** 16
> 
> **Explanation:**
> 
> The bookstore owner keeps themselves not grumpy for the last 3 minutes.
> 
> The maximum number of customers that can be satisfied = 1 + 1 + 1 + 1 + 7 + 5 = 16.

**Example 2:**

> **Input:** customers = [1], grumpy = [0], minutes = 1
> 
> **Output:** 1

**Constraints:**

-   `n == customers.length == grumpy.length`
-   `1 <= minutes <= n <= 2 * 104`
-   `0 <= customers[i] <= 1000`
-   `grumpy[i]` is either `0` or `1`.

---
```java
// Java 3ms(Beats 89.12%), Time O(N), Space O(1)
class Solution {
    public int maxSatisfied(int[] customers, int[] grumpy, int minutes) {
        int ret = 0;
        int bonus = 0;
        int maxBonus = 0;
        for (int i = 0; i < customers.length; i++)
        {
            if (grumpy[i] == 0)
                ret += customers[i];
            else
                bonus += customers[i];
            
            if (i - minutes >= 0 && grumpy[i - minutes] == 1)
                bonus -= customers[i - minutes];
                
            maxBonus = Math.max(maxBonus, bonus); 
        }

        return ret + maxBonus;
    }
}
```
---

本題的意思是可以給你一個固定長度的滑窗，滑窗內的滿意度可以累積（不管是否grumpy）。求某個滑窗位置時，整體滿意度的最大值。

先算出`sum`的基本盤，就是在沒有滑窗覆蓋的情況下，可以得到的滿意度總和。

然後考慮一個長度為X的滑窗內，我們可能可以額外增加一些滿意度。滑動視窗每一次移動，會加入一個元素`customers[i]`，考察它對應的`grumpy[i]`是否是`1`，是的話那就是額外增加的滿意度，需要加入`sum`。同時也會有一個元素`customers[i-X]`離開滑窗，同樣考察它對應的`grumpy[i+X]`是否是`1`，是的話我們就要退回額外的滿意度，從`sum`中扣除。

整個滑動過程中出現的最大`sum`，就是答案


###### tags: `Leetcode` `Two Pointers` `Sliding window`