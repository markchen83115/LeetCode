# Leetcode - 1851. Minimum Interval to Include Each Query (H)

[Leetcode](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

You are given a 2D integer array `intervals`, where `intervals[i] = [lefti, righti]` describes the `ith` interval starting at `lefti` and ending at `righti` **(inclusive)**. The **size** of an interval is defined as the number of integers it contains, or more formally `righti - lefti + 1`.

You are also given an integer array `queries`. The answer to the `jth` query is the **size of the smallest interval** `i` such that `lefti <= queries[j] <= righti`. If no such interval exists, the answer is `-1`.

Return _an array containing the answers to the queries_.

**Example 1:**

> **Input:** intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
> **Output:** [3,3,1,4]
> **Explanation:** The queries are processed as follows:
> - Query = 2: The interval [2,4] is the smallest interval containing 2. The answer is 4 - 2 + 1 = 3.
> - Query = 3: The interval [2,4] is the smallest interval containing 3. The answer is 4 - 2 + 1 = 3.
> - Query = 4: The interval [4,4] is the smallest interval containing 4. The answer is 4 - 4 + 1 = 1.
> - Query = 5: The interval [3,6] is the smallest interval containing 5. The answer is 6 - 3 + 1 = 4.

**Example 2:**

> **Input:** intervals = [[2,3],[2,5],[1,8],[20,25]], queries = [2,19,5,22]
> **Output:** [2,-1,4,6]
> **Explanation:** The queries are processed as follows:
> - Query = 2: The interval [2,3] is the smallest interval containing 2. The answer is 3 - 2 + 1 = 2.
> - Query = 19: None of the intervals contain 19. The answer is -1.
> - Query = 5: The interval [2,5] is the smallest interval containing 5. The answer is 5 - 2 + 1 = 4.
> - Query = 22: The interval [20,25] is the smallest interval containing 22. The answer is 25 - 20 + 1 = 6.

**Constraints:**

-   `1 <= intervals.length <= 105`
-   `1 <= queries.length <= 105`
-   `intervals[i].length == 2`
-   `1 <= lefti <= righti <= 107`
-   `1 <= queries[j] <= 107`

---
```java
// Java 105ms(Beats 62.60%), Time O(NlogN), Space O(N)
class Solution {
    public int[] minInterval(int[][] intervals, int[] queries) {
        int n = queries.length;
        int m = intervals.length;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]); // len, endTime
        int[][] query = new int[n][2];
        int[] ret = new int[n];
        for (int i = 0; i < n; i++)
            query[i] = new int[] {queries[i], i};
        
        Arrays.sort(query, (a, b) -> a[0] - b[0]);
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int j = 0;
        for (int i = 0; i < n; i++)
        {
            int curr = query[i][0];
            int index = query[i][1];
            // add interval into queue
            while (j < m && intervals[j][0] <= curr)
            {
                pq.offer( new int[] {intervals[j][1] - intervals[j][0] + 1, intervals[j][1]});
                j++;
            }
                
            
            // remove interval into queue
            while (!pq.isEmpty() && pq.peek()[1] < curr)
                pq.poll();
            
            if (!pq.isEmpty())
                ret[index] = pq.peek()[0];
            else
                ret[index] = -1;
        }

        return ret;
    }
}
```
---

對於`offline querying`的問題, 我們首先想到是否可以調整`query`的順序來使得問題簡化

對於`query`內的每一個`q`, 我們要找到所有區間都包括`q`的`interval`, 使得時間複雜度達到了O(N^2)

我們目標是隨著q的遍歷，只需對考察的interval集合進行增補(引進新的、捨棄舊的), 使得`interval`的總遍歷是線性時間。

我們想像，對於`q`，符合題意的`interval`肯定要求`startTime`必須早於`q`
有個想法是：將所有`startTime <= q`的`interval`放入一個`PQ`，每個`interval`包含兩個屬性：`{duration, endTime}`
我們貪心地看`PQ`頂端的`interval`，它的`duration`一定是當前最小的，但是`endTime`不一定符合要求(即`endTime <= q`)
但是沒關係，不符合條件的就從`PQ`頂端彈出，直至`PQ`頂端的`interval`的`endTime`符合要求，那麼自然它的`duration`也是在符合條件的`interval`裡最小的

這其中隱含著一個考慮，那就是`endTime`不符合要求的`interval`，對於更靠後的`query`肯定也不會符合要求，所以可以放心捨去。

本題的時間複雜度是`O(NlogN)`，其中`N`是事件的個數
因為每個`interval`只會進入`PQ`一次，出`PQ`一次，每次插入/彈出都是`logN`的時間。這與`query`的數量沒有關係。


###### tags: `Leetcode` `Priority Queue` `Sort+PQ`