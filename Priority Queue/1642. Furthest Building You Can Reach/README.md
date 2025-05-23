# Leetcode - 1642. Furthest Building You Can Reach (H-)

[Leetcode](https://leetcode.com/problems/furthest-building-you-can-reach/)

You are given an integer array `heights` representing the heights of buildings, some `bricks`, and some `ladders`.

You start your journey from building `0` and move to the next building by possibly using bricks or ladders.

While moving from building `i` to building `i+1` (**0-indexed**),

-   If the current building's height is **greater than or equal** to the next building's height, you do **not** need a ladder or bricks.
-   If the current building's height is **less than** the next building's height, you can either use **one ladder** or `(h[i+1] - h[i])` **bricks**.

_Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/27/q4.gif)
```
Input: heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
Output: 4
Explanation: Starting at building 0, you can follow these steps:
- Go to building 1 without using ladders nor bricks since 4 >= 2.
- Go to building 2 using 5 bricks. You must use either bricks or ladders because 2 < 7.
- Go to building 3 without using ladders nor bricks since 7 >= 6.
- Go to building 4 using your only ladder. You must use either bricks or ladders because 6 < 9.
It is impossible to go beyond building 4 because you do not have any more bricks or ladders.
```
**Example 2:**
```
Input: heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
Output: 7
```
**Example 3:**
```
Input: heights = [14,3,19,3], bricks = 17, ladders = 0
Output: 3
```
**Constraints:**

-   `1 <= heights.length <= 105`
-   `1 <= heights[i] <= 106`
-   `0 <= bricks <= 109`
-   `0 <= ladders <= heights.length`

---
```java
// Java 19ms (Beats 69.61%), Time O(NlogN), Space O(N)
class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        int ret = 0;
        for (int i = 0; i < heights.length - 1; i++)
        {
            int diff = heights[i+1] - heights[i];
            if (diff <= 0)
                continue;
            
            pq.offer(diff);
            
            if (pq.size() > ladders)
                bricks -= pq.poll();
            
            if (bricks < 0)
                return i;
        }

        return heights.length - 1;
    }
}
```

---

遇到高度差大的盡量用梯子，遇到高度差小的用磚頭

只考慮需要爬升的建築物(下降的建築物可以忽略)
如果需要爬升的樓的數目`<=ladders`，那麼是一定是可以到達的
如果我們需要爬升`ladders+1`棟大樓的時候，其中必然會有一次需要用到磚頭
那我們是在爬哪一棟樓的時候用磚頭呢？顯然，是高度差最小的那棟樓

一個很重要的結論：無論未來會遇到什麼樣的樓（或高或低），這幢依靠磚頭去爬的樓一定不會改變當初的決策
為什麼？因為已經有ladders棟樓的跨度比它大了，無論如何，這幢樓都不會有資格去使用梯子

具體的演算法是：我們逐一遍歷樓層，將跨度依序放入一個優先隊列中
如果隊列的元素數目大於ladders，那麼目前最小的元素必然需要用磚頭來實現（隱含的意思就是其他元素可以用梯子來實現）
於是磚頭的總數減去該跨度，並將該跨度從優先隊列中彈出
前進的過程中不斷重複這個過程，直到磚塊不夠用為止


###### tags: `Leetcode` `Priority Queue`