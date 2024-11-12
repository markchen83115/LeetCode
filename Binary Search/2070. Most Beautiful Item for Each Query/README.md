# Leetcode - 2070. Most Beautiful Item for Each Query (M)

[Leetcode](https://leetcode.com/problems/most-beautiful-item-for-each-query/)

You are given a 2D integer array `items` where `items[i] = [pricei, beautyi]` denotes the **price** and **beauty** of an item respectively.

You are also given a **0-indexed** integer array `queries`. For each `queries[j]`, you want to determine the **maximum beauty** of an item whose **price** is **less than or equal** to `queries[j]`. If no such item exists, then the answer to this query is `0`.

Return _an array _`answer`_ of the same length as _`queries`_ where _`answer[j]`_ is the answer to the _`jth`_ query_.

**Example 1:**

> **Input:** items = [[1,2],[3,2],[2,4],[5,6],[3,5]], queries = [1,2,3,4,5,6]
> **Output:** [2,4,5,5,6,6]
> **Explanation:**
> - For queries[0]=1, [1,2] is the only item which has price <= 1. Hence, the answer for this query is 2.
> - For queries[1]=2, the items which can be considered are [1,2] and [2,4]. 
>   The maximum beauty among them is 4.
> - For queries[2]=3 and queries[3]=4, the items which can be considered are [1,2], [3,2], [2,4], and [3,5].
>   The maximum beauty among them is 5.
> - For queries[4]=5 and queries[5]=6, all items can be considered.
>   Hence, the answer for them is the maximum beauty of all items, i.e., 6.

**Example 2:**

> **Input:** items = [[1,2],[1,2],[1,3],[1,4]], queries = [1]
> **Output:** [4]
> **Explanation:** 
> The price of every item is equal to 1, so we choose the item with the maximum beauty 4. 
> Note that multiple items can have the same price and/or beauty.  

**Example 3:**

> **Input:** items = [[10,1000]], queries = [5]
> **Output:** [0]
> **Explanation:**
> No item has a price less than or equal to 5, so no item can be chosen.
> Hence, the answer to the query is 0.

**Constraints:**

-   `1 <= items.length, queries.length <= 105`
-   `items[i].length == 2`
-   `1 <= pricei, beautyi, queries[j] <= 109`

---
```java
// Java TreeMap 35ms(Beats 98.55%), Time O(NlogN), Space O(N)
class Solution {
    public int[] maximumBeauty(int[][] items, int[] queries) {
        TreeMap<Integer, Integer> map = new TreeMap<>();
        int n = queries.length;
        int[] ret = new int[n];
        Arrays.sort(items, (a, b) -> a[0] - b[0]);

        int max = 0;
        map.put(0, 0);
        for (int[] item : items)
        {
            if (item[1] < max)
                continue;
            max = item[1];
            map.put(item[0], item[1]);
        }

        for (int i = 0; i < n; i++)
        {
            // map.floorEntry(queries[i]).getValue();
            int key = map.floorKey(queries[i]);
            ret[i] = map.get(key);
        }

        return ret;
    }
}
```
```java
// Java BinarySearch 45ms(Beats 59.42%), Time O(NlogN), Space O(1)
class Solution {
    public int[] maximumBeauty(int[][] items, int[] queries) {
        int n = queries.length;
        int[] ret = new int[n];
        Arrays.sort(items, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);

        int max = 0;
        for (int i = 0; i < items.length; i++)
        {
            max = Math.max(max, items[i][1]);
            items[i][1] = max;
        }

        for (int i = 0; i < n; i++)
            ret[i] = binarySearch(items, queries[i]);

        return ret;
    }

    int binarySearch(int[][] items, int k)
    {
        int l = 0, r = items.length - 1;
        while (l < r)
        {
            int mid = r - (r - l) / 2;
            if (items[mid][0] <= k)
                l = mid;
            else
                r = mid - 1;
        }

        if (k < items[l][0])
            return 0;

        return items[l][1];
    }
}
```
---

將items以`items[i][0]`以小到大排序
遍歷items, 並每次計算當前最大值
若目前`items[i][1] <= max`, 代表`item[i][1]`不可能為當前最大的beauty值
只有`items[i][1] > max`時, `item[i][1]`才可能是當前最大的beauty值

再利用TreeMap的floorKey(k)來取得在`<= k`的所有key的最大值
即為`queries[i]`的解


###### tags: `Leetcode` `Binary Search`