# Leetcode - 2406. Divide Intervals Into Minimum Number of Groups (M+)

[Leetcode](https://leetcode.com/problems/divide-intervals-into-minimum-number-of-groups/)

You are given a 2D integer array `intervals` where `intervals[i] = [lefti, righti]` represents the **inclusive** interval `[lefti, righti]`.

You have to divide the intervals into one or more **groups** such that each interval is in **exactly** one group, and no two intervals that are in the same group **intersect** each other.

Return _the **minimum** number of groups you need to make_.

Two intervals **intersect** if there is at least one common number between them. For example, the intervals `[1, 5]` and `[5, 8]` intersect.

**Example 1:**
```
Input: intervals = [[5,10],[6,8],[1,5],[2,3],[1,10]]
Output: 3
Explanation: We can divide the intervals into the following groups:
- Group 1: [1, 5], [6, 8].
- Group 2: [2, 3], [5, 10].
- Group 3: [1, 10].
It can be proven that it is not possible to divide the intervals into fewer than 3 groups.
```
**Example 2:**
```
Input: intervals = [[1,3],[5,6],[8,10],[11,13]]
Output: 1
Explanation: None of the intervals overlap, so we can put all of them in one group.
```
**Constraints:**

-   `1 <= intervals.length <= 105`
-   `intervals[i].length == 2`
-   `1 <= lefti <= righti <= 106`

---
```java
// Java 99ms(Beats 33.15%), Time O(NlogN), Space O(N)
class Solution {
    public int minGroups(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        PriorityQueue<Integer> pq = new PriorityQueue<>();   // (end)
        for (int[] interval : intervals)
        {
            if (!pq.isEmpty() && pq.peek() < interval[0])
                pq.poll();
            
            pq.offer(interval[1]);
        }

        return pq.size();
    }
}
```

```java
// Java 136ms(Beats 14.13%), Time O(NlogN), Space O(N)
class Solution {
    public int minGroups(int[][] intervals) {
        int ret = 0;
        int cur = 0;
        int n = intervals.length;
        int[][] A = new int[n * 2][2];
        for (int i = 0; i < n; i++)
        {
            A[i * 2] = new int[] {intervals[i][0], 1};
            A[i * 2 + 1] = new int[] {intervals[i][1] + 1, -1};
        }

        Arrays.sort(A, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        for (int[] a : A)
            ret = Math.max(ret, cur += a[1]);
        
        return ret;
    }
}
```
```java
// Java 32ms(Beats 93.21%), Time O(NlogN), Space O(N)
class Solution {
    public int minGroups(int[][] intervals) {
        int ret = 0;
        int[] time = new int[1000005];
        for (int[] i : intervals)
        {
            time[i[0]]++;
            time[i[1] + 1]--;
        }

        int count = 0;
        for (int t : time)
            ret = Math.max(ret, count += t);
        
        return ret;
    }
}
```
---

1. PQ貪心
	以開始時間為主排序, 並記錄結束時間
	若下一個開始時間早於最早的結束時間, 代表需要新開一個房間
	若晚於最早的結束時間, 代表可重複利用該房間
	
	因此維護一個`PQ`, 將結束時間放入, 
	當下一個`interval`時, 檢查最早的結束時間是否已結束, 
	結束則彈出, 並放入新的結束時間
	答案為`PQ.size()`
	
2. 掃描線
	使用掃描線可輕鬆得知最多有多少interval存在
	
	注意: 此題不允許左右端點重合, 因此將右端點+1
	即為`left`時加入會議, `right+1`時退出會議
	

###### tags: `Leetcode` `Priority Queue` `Sort+PQ`