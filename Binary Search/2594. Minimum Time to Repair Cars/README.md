# Leetcode - 2594. Minimum Time to Repair Cars (M)

[Leetcode](https://leetcode.com/problems/minimum-time-to-repair-cars/)

You are given an integer array `ranks` representing the **ranks** of some mechanics. ranksi is the rank of the ith mechanic. A mechanic with a rank `r` can repair n cars in `r * n2` minutes.

You are also given an integer `cars` representing the total number of cars waiting in the garage to be repaired.

Return _the **minimum** time taken to repair all the cars._

**Note:** All the mechanics can repair the cars simultaneously.

**Example 1:**

> **Input:** ranks = [4,2,3,1], cars = 10
> **Output:** 16
> **Explanation:** 
> - The first mechanic will repair two cars. The time required is 4 * 2 * 2 = 16 minutes.
> - The second mechanic will repair two cars. The time required is 2 * 2 * 2 = 8 minutes.
> - The third mechanic will repair two cars. The time required is 3 * 2 * 2 = 12 minutes.
> - The fourth mechanic will repair four cars. The time required is 1 * 4 * 4 = 16 minutes.
> It can be proved that the cars cannot be repaired in less than 16 minutes.

**Example 2:**

> **Input:** ranks = [5,1,8], cars = 6
> **Output:** 16
> **Explanation:** 
> - The first mechanic will repair one car. The time required is 5 * 1 * 1 = 5 minutes.
> - The second mechanic will repair four cars. The time required is 1 * 4 * 4 = 16 minutes.
> - The third mechanic will repair one car. The time required is 8 * 1 * 1 = 8 minutes.
> It can be proved that the cars cannot be repaired in less than 16 minutes.

**Constraints:**

-   `1 <= ranks.length <= 105`
-   `1 <= ranks[i] <= 100`
-   `1 <= cars <= 106`

---
```java
// Java 19ms(Beats 85.75%), Time O(NlogN), Space O(1)
class Solution {
    public long repairCars(int[] ranks, int cars) {
        long ret = 0;
        long l = 0, r = (long) 1e14; 
        while (l < r) {
            long mid = l + (r - l) / 2;
            if (isOK(ranks, cars, mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    boolean isOK(int[] ranks, int cars, long time)
    {
        for (int r : ranks)
        {
            cars -= (int) Math.sqrt(time / r);
            if (cars <= 0)
                return true;
        }

        return false;
    }
}
```
---

最基本的二分搜值。猜測一個時間t，看看在這個時間內所有人修車數目的總和是否大於等於cars。是的話試圖減小t，否則的話試圖增加t，直至收斂。

對於給定的t，每個人的修車數量就是`sqrt(t/r)`.


###### tags: `Leetcode` `Binary Search` `Binary Search by Value`