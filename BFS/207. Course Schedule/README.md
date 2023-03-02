# Leetcode - 207. Course Schedule (H-)

[Leetcode](https://leetcode.com/problems/course-schedule/description/)

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

-   For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```
**Example 2:**
```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```
**Constraints:**

-   `1 <= numCourses <= 2000`
-   `0 <= prerequisites.length <= 5000`
-   `prerequisites[i].length == 2`
-   `0 <= ai, bi < numCourses`
-   All the pairs prerequisites[i] are **unique**.

---
```java
// Java DFS
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] visited = new int[numCourses];
        ArrayList<Integer>[] next = new ArrayList[numCourses]; // next[i]: course i is prerequisite of next[i]
        for (int i = 0; i < numCourses; i++) {
            next[i] = new ArrayList<Integer>();
        }
        for (int[] pre : prerequisites) {
            next[pre[1]].add(pre[0]);
        }

        for (int i = 0; i < numCourses; i++) {
            if (dfs(i, visited, next) == false)
                return false;
        }
        return true;
    }

    public boolean dfs(int i, int[] visited, ArrayList<Integer>[] next) {
        if (visited[i] == 1) return true;
        if (visited[i] == 2) return false;
        
        visited[i] = 2;
        for (int pre : next[i]) {
            if (dfs(pre, visited, next) == false)
                return false;
        }
        visited[i] = 1;
        return true;
    }
}
```

```java
// Java BFS
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        Queue<Integer> queue = new LinkedList<Integer>();
        int[] inDegree = new int[numCourses];
        ArrayList<Integer>[] next = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) {
            next[i] = new ArrayList<Integer>();
        }
        for (int[] pre : prerequisites) {
            next[pre[1]].add(pre[0]);
            inDegree[pre[0]]++;
        }
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0)
                queue.offer(i);
        }
        int counts = 0;
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            counts++;
            for (int pre : next[cur]) {
                if (--inDegree[pre] == 0)
                    queue.offer(pre);
            }
        }

        return counts == numCourses;
    }
}
```

---

[以下取自wisdompeak - 207.Course-Schedule](https://github.com/wisdompeak/LeetCode/tree/master/BFS/207.Course-Schedule)

**問題是在尋找此有向圖是否有環(cycle)**

> DFS

DFS的基本思想是從任意一個未訪問過的節點開始做DFS的遍歷。
如果在某條支路的遍歷過程中, 沒有遍歷到出度(OutDegree)為0的端點, 遇到了任何在這條支路中已經訪問過的節點，那麼就能判斷成環。

注意:
**遇到了任何在這條支路中已經訪問過的節點** 和 **遇到了任何已經訪問過的節點**，是不同的概念。

比如:
```
1 -> 2 -> 3 -> 4 
          ^
5 -> 6 -> 7 -> 8 
          ^____|
```
我們從`1`開始依次訪問`1->2->3->4`，然後遍歷結束。
然後從`5`開始依次訪問`5->6->7->3`的時候，`3`已經被訪問過了,
但是這不會誤判成環, 因為`3`並不是在當前未完待續的支路中。

我們再看`5->6->7->8->7`這條線路
此時的`7`已經被這條支路訪問過，並且這條支路並沒有走到底，這個時候就應該判斷成環。

所以我們需要標記兩種`visited[i]`。
如果節點i已經在其他遍歷到底的支路中被訪問過了，標記1
如果節點i是在當前未完待續的支路中被訪問過了，標記2
只有在遍歷過程中遇到了2，才算是判斷有環

那麼是什麼時候標記1什麼時候標記2呢？
**方法：在某條DFS的路徑上，第一次遇到的節點i的時候標記2, 在回溯返回節點i的時候標記1**
因為能成功返回的話，說明後續的節點都沒有環，都是死胡同
此後任何任何入度(InDegree)指向這個節點i的話，我們都不用擔心後續的遍歷會遇到環

核心的dfs程式碼：

```cpp
// 從cur出發, 有環 = false, 沒環 = true
bool dfs(int cur) 
{
    if (visited[next]==1) return true;
    if (visited[next]==2) return false;
    visited[cur] = 2;
    for (int next: graph[cur])
    {
        if (dfs(next)==false)  return false;
    }
    visited[cur] = 1;
    return true;
}
```

> BFS

BFS的算法思想是拓撲排序：從外圍往核心前進

我們每次在圖中找入度(InDegree)為0的點，然後移除
如果最後沒有入度(InDegree)為0的點，但是圖中仍有點存在
那麼這些剩下來的點一定是交錯成環的。


###### tags: `Leetcode` `BFS` `DFS` `拓撲排序`
