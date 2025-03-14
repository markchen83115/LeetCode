# Leetcode - 2940. Find Building Where Alice and Bob Can Meet (H)

[Leetcode](https://leetcode.com/problems/find-building-where-alice-and-bob-can-meet/)

You are given a **0-indexed** array `heights` of positive integers, where `heights[i]` represents the height of the `ith` building.

If a person is in building `i`, they can move to any other building `j` if and only if `i < j` and `heights[i] < heights[j]`.

You are also given another array `queries` where `queries[i] = [ai, bi]`. On the `ith` query, Alice is in building `ai` while Bob is in building `bi`.

Return _an array_ `ans` _where_ `ans[i]` _is **the index of the leftmost building** where Alice and Bob can meet on the_ `ith` _query_. _If Alice and Bob cannot move to a common building on query_ `i`, _set_ `ans[i]` _to_ `-1`.

**Example 1:**

> **Input:** heights = [6,4,8,5,2,7], queries = [[0,1],[0,3],[2,4],[3,4],[2,2]]
> **Output:** [2,5,-1,5,2]
> **Explanation:** In the first query, Alice and Bob can move to building 2 since heights[0] < heights[2] and heights[1] < heights[2]. 
> In the second query, Alice and Bob can move to building 5 since heights[0] < heights[5] and heights[3] < heights[5]. 
> In the third query, Alice cannot meet Bob since Alice cannot move to any other building.
> In the fourth query, Alice and Bob can move to building 5 since heights[3] < heights[5] and heights[4] < heights[5].
> In the fifth query, Alice and Bob are already in the same building.  
> For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet.
> For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.

**Example 2:**

> **Input:** heights = [5,3,8,2,6,1,4,6], queries = [[0,7],[3,5],[5,2],[3,0],[1,6]]
> **Output:** [7,6,-1,4,6]
> **Explanation:** In the first query, Alice can directly move to Bob's building since heights[0] < heights[7].
> In the second query, Alice and Bob can move to building 6 since heights[3] < heights[6] and heights[5] < heights[6].
> In the third query, Alice cannot meet Bob since Bob cannot move to any other building.
> In the fourth query, Alice and Bob can move to building 4 since heights[3] < heights[4] and heights[0] < heights[4].
> In the fifth query, Alice can directly move to Bob's building since heights[1] < heights[6].
> For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet.
> For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.

**Constraints:**

-   `1 <= heights.length <= 5 * 104`
-   `1 <= heights[i] <= 109`
-   `1 <= queries.length <= 5 * 104`
-   `queries[i] = [ai, bi]`
-   `0 <= ai, bi <= heights.length - 1`

---
```java
// Java 107ms(Beats 12.82%), Time O(NlogN), Space O(N)
class Solution {
    public int[] leftmostBuildingQueries(int[] heights, int[][] queries) {
        int n = queries.length;
        int[][] qrys = new int[n][];
        int[] ret = new int[n];
        // 排序query, 以[a, b, index] b越大越前面
        for (int i = 0; i < n; i++)
            qrys[i] = new int[] {Math.min(queries[i][0], queries[i][1]), Math.max(queries[i][0], queries[i][1]), i};
        Arrays.sort(qrys, (a, b) -> b[1] - a[1]);

        // 嚴格遞增的容器
        int i = heights.length - 1;
        TreeMap<Integer, Integer> map = new TreeMap<>();    // height, index 
        for (int[] q : qrys)
        {
            int a = q[0], b = q[1], index = q[2];
            while (b <= i)
            {
                // 處理嚴格遞增
                while (!map.isEmpty() && heights[i] >= map.firstKey())
                    map.remove(map.firstKey());
                map.put(heights[i], i);
                i--;
            }

            // 可直接得到答案
            if (a == b || heights[a] < heights[b])
            {
                ret[index] = b;
                continue;
            }

            int maxHeight = Math.max(heights[a], heights[b]);
            Integer meet = map.ceilingKey(maxHeight + 1);   // 要找 > maxHeight
            if (meet == null)
                ret[index] = -1;
            else
                ret[index] = map.get(meet);
        }

        return ret;
    }
}
```
---

參考[【每日一题】LeetCode 2940. Find Building Where Alice and Bob Can Meet - YouTube](https://youtu.be/FHgwJGZN9x0)


###### tags: `Leetcode` `Sorted Container` `Sorted_Container w/ monotonic mapping values`