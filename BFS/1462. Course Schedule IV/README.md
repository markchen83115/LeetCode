# Leetcode - 1462. Course Schedule IV (M)

[Leetcode](https://leetcode.com/problems/course-schedule-iv/)

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `ai` first if you want to take course `bi`.

-   For example, the pair `[0, 1]` indicates that you have to take course `0` before you can take course `1`.

Prerequisites can also be **indirect**. If course `a` is a prerequisite of course `b`, and course `b` is a prerequisite of course `c`, then course `a` is a prerequisite of course `c`.

You are also given an array `queries` where `queries[j] = [uj, vj]`. For the `jth` query, you should answer whether course `uj` is a prerequisite of course `vj` or not.

Return _a boolean array _`answer`_, where _`answer[j]`_ is the answer to the _`jth`_ query._

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/05/01/courses4-1-graph.jpg)
> 
> **Input:** numCourses = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
> **Output:** [false,true]
> **Explanation:** The pair [1, 0] indicates that you have to take course 1 before you can take course 0.
> Course 0 is not a prerequisite of course 1, but the opposite is true.

**Example 2:**

> **Input:** numCourses = 2, prerequisites = [], queries = [[1,0],[0,1]]
> **Output:** [false,false]
> **Explanation:** There are no prerequisites, and each course is independent.

**Example 3:**

> ![](https://assets.leetcode.com/uploads/2021/05/01/courses4-3-graph.jpg)
> 
> **Input:** numCourses = 3, prerequisites = [[1,2],[1,0],[2,0]], queries = [[1,0],[1,2]]
> **Output:** [true,true]

**Constraints:**

-   `2 <= numCourses <= 100`
-   `0 <= prerequisites.length <= (numCourses * (numCourses - 1) / 2)`
-   `prerequisites[i].length == 2`
-   `0 <= ai, bi <= numCourses - 1`
-   `ai != bi`
-   All the pairs `[ai, bi]` are **unique**.
-   The prerequisites graph has no cycles.
-   `1 <= queries.length <= 104`
-   `0 <= ui, vi <= numCourses - 1`
-   `ui != vi`

---
```java
// Java 45ms(Beats 54.56%), Time O(N), Space O(N)
class Solution {
    public List<Boolean> checkIfPrerequisite(int n, int[][] prerequisites, int[][] queries) {
        HashSet<Integer>[] preReq = new HashSet[n];
        ArrayList<Integer>[] next = new ArrayList[n];
        int[] inDegree = new int[n];

        for (int i = 0; i < n; i++)
        {
            preReq[i] = new HashSet<>();
            next[i] = new ArrayList<>();
        }
        
        for (int[] p : prerequisites)
        {
            next[p[0]].add(p[1]);
            inDegree[p[1]]++;
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++)
            if (inDegree[i] == 0)
                queue.offer(i);

        while (!queue.isEmpty())
        {
            int curr = queue.poll();
            for (int nxt : next[curr])
            {
                preReq[nxt].add(curr);
                for (int pre : preReq[curr])
                    preReq[nxt].add(pre);

                if (--inDegree[nxt] == 0)
                    queue.offer(nxt);
            }
        }

        List<Boolean> ret = new ArrayList<>();
        for (int[] q : queries)
            ret.add(preReq[q[1]].contains(q[0]));
        
        return ret;
    }
}
```
---

考慮到`n <= 100`，即使將每個節點的所有先修課程都記錄下來，時間複雜度`O(N^2)`也是可以接受的。
於是本題就是常規的拓樸排序演算法，需要特別處理的是：每次從`curr`拓展到下一個`next`節點時，要把`curr`的所有先修課程都複製一遍給`next`。
至於資料結構，顯然用`Set`來實現去重和查詢`query`都很方便。



###### tags: `Leetcode` `BFS` `拓撲排序`