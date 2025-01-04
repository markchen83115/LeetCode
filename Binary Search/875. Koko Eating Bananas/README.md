# Leetcode - 875. Koko Eating Bananas (M-)

[Leetcode](https://leetcode.com/problems/koko-eating-bananas/)

Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.

**Example 1:**

> **Input:** piles = [3,6,7,11], h = 8
> **Output:** 4

**Example 2:**

> **Input:** piles = [30,11,23,4,20], h = 5
> **Output:** 30

**Example 3:**

> **Input:** piles = [30,11,23,4,20], h = 6
> **Output:** 23

**Constraints:**

-   `1 <= piles.length <= 104`
-   `piles.length <= h <= 109`
-   `1 <= piles[i] <= 109`

---
```java
// Java 6ms(Beats 99.43%), Time O(NlogN), Space O(1)
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int l = 1, r = (int)1e9;
        while (l < r)
        {
            int mid = l + (r - l) / 2;
            if (isOK(piles, h, mid))
                r = mid;
            else
                l = mid + 1;
        }

        return l;
    }

    boolean isOK(int[] piles, int h, int k)
    {
        for (int p : piles)
        {
            h -= (p + k - 1) / k;
            if (h < 0)
                return false;
        }
        return true;
    }
}
```
---

很明顯當吃的速度k越大, 越能在h小時內吃完, 而k越小, 越難在h小時內吃完
用二分搜值去檢查當速度k時, 能否在h小時內吃完
若可以吃完, 則在降低速度k, 反之增加速度k


###### tags: `Leetcode` `Binary Search` `Binary Search by Value`