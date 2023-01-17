# Leetcode — 57. Insert Interval (M)

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` *after the insertion*.

**Example 1:**
```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]  
Output: [[1,5],[6,9]]
```
**Example 2:**
```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]  
Output: [[1,2],[3,10],[12,16]]  
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```
**Constraints:**

-   `0 <= intervals.length <= 104`
-   `intervals[i].length == 2`
-   `0 <= starti <= endi <= 105`
-   `intervals` is sorted by `starti` in **ascending** order.
-   `newInterval.length == 2`
-   `0 <= start <= end <= 105`

---

```java
//Java  
class Solution {  
    public int[][] insert(int[][] intervals, int[] newInterval) {  
        List<int[]> ret = new ArrayList<>();  
        int i = 0;  
        int start = newInterval[0], end = newInterval[1];  
          
        // < newInterval  
        while (i < intervals.length && intervals[i][1] < start) {  
            ret.add(intervals[i++]);  
        }  
          
        // merge with newInterval  
        while(i < intervals.length && intervals[i][0] <= end) {  
            start = Math.min(start, intervals[i][0]);  
            end = Math.max(end, intervals[i][1]);  
            i++;  
        }  
        ret.add(new int[] {start, end});  
          
        // > newInterval  
        while(i < intervals.length) {  
            ret.add(intervals[i++]);  
        }  
      
        return ret.toArray(new int[ret.size()][]);  
    }  
}
```
```csharp
//c#  
public class Solution {  
    public int[][] Insert(int[][] intervals, int[] newInterval) {  
        int i = 0, n = intervals.Length;  
        int start = newInterval[0], end = newInterval[1];  
        List<int[]> ret = new List<int[]>();  
          
        // < newInterval  
        while (i < n && intervals[i][1] < start) {  
            ret.Add(intervals[i]);  
            i++;  
        }  
          
        //merge with newInterval  
        while (i < n && intervals[i][0] <= end) {  
            start = Math.Min(start, intervals[i][0]);  
            end = Math.Max(end, intervals[i][1]);  
            i++;  
        }  
        ret.Add(new int[] {start, end});  
          
        // > newInterval  
        while (i < n) {  
            ret.Add(intervals[i]);  
            i++;  
        }  
          
        return ret.ToArray();  
    }  
}
```


---
注意merge with newInterval的條件  
`intervals[i][0] ≤ newInterval[1]` 即代表需要merge  
因為比newInterval小的已被剔除  
ex: newInterval = \[4,6\], 則\[4, …\], \[5, …\], \[6, …\]都需要merge

因為後面的條件是`intervals[i][1] ≤ interval[0]`, 都比newInterval大  
ex: newInterval = \[4,6\], 則 \[>6, …\] 都比newInterval大


###### tags: `Leetcode` `掃描線/差分陣列`