# Leetcode - 1014. Best Sightseeing Pair (M)

[Leetcode](https://leetcode.com/problems/best-sightseeing-pair/)

You are given an integer array `values` where values[i] represents the value of the `ith` sightseeing spot. Two sightseeing spots `i` and `j` have a **distance** `j - i` between them.

The score of a pair (`i < j`) of sightseeing spots is `values[i] + values[j] + i - j`: the sum of the values of the sightseeing spots, minus the distance between them.

Return _the maximum score of a pair of sightseeing spots_.

**Example 1:**

> **Input:** values = [8,1,5,2,6]
> **Output:** 11
> **Explanation:** i = 0, j = 2, values[i] + values[j] + i - j = 8 + 5 + 0 - 2 = 11

**Example 2:**

> **Input:** values = [1,2]
> **Output:** 2

**Constraints:**

-   `2 <= values.length <= 5 * 104`
-   `1 <= values[i] <= 1000`

---
```java
// Java 3ms(Beats 97.03%), Time O(N), Space O(N)
class Solution {
    public int maxScoreSightseeingPair(int[] values) {
        int ret = 0;
        int prefixMax = 0;
        for (int value : values)
        {
            prefixMax--;
            ret = Math.max(ret, prefixMax + value);
            prefixMax = Math.max(prefixMax, value);
        }

        return ret;
    }
}
```
---

把 score 拆解成：`（vi + i) + (vj - j)`
那我今天站在`j`，想要讓 score 最大，就是要找一個最大的 `vi + i` 給他，所以上面問題的答案就是： 
1. 方向：往前找
2. 某個資訊：最大值

轉換成功之後我就可以： 
1. 跑一次 values，紀錄每個位置當下的最大值
2. 再跑一次 values，配合最大值去計算最大分數


###### tags: `Leetcode` `Greedy`