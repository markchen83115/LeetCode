# Leetcode - 1337. The K Weakest Rows in a Matrix (E)

[Leetcode](https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/)

You are given an `m x n` binary matrix `mat` of `1`'s (representing soldiers) and `0`'s (representing civilians). The soldiers are positioned **in front** of the civilians. That is, all the `1`'s will appear to the **left** of all the `0`'s in each row.

A row `i` is **weaker** than a row `j` if one of the following is true:

-   The number of soldiers in row `i` is less than the number of soldiers in row `j`.
-   Both rows have the same number of soldiers and `i < j`.

Return _the indices of the _`k`_ **weakest** rows in the matrix ordered from weakest to strongest_.

**Example 1:**
```
Input: mat = 
[[1,1,0,0,0],
 [1,1,1,1,0],
 [1,0,0,0,0],
 [1,1,0,0,0],
 [1,1,1,1,1]], 
k = 3
Output: [2,0,3]
Explanation: 
The number of soldiers in each row is: 
- Row 0: 2 
- Row 1: 4 
- Row 2: 1 
- Row 3: 2 
- Row 4: 5 
The rows ordered from weakest to strongest are [2,0,3,1,4].
```
**Example 2:**
```
Input: mat = 
[[1,0,0,0],
 [1,1,1,1],
 [1,0,0,0],
 [1,0,0,0]], 
k = 2
Output: [0,2]
Explanation: 
The number of soldiers in each row is: 
- Row 0: 1 
- Row 1: 4 
- Row 2: 1 
- Row 3: 1 
The rows ordered from weakest to strongest are [0,2,3,1].
```
**Constraints:**

-   `m == mat.length`
-   `n == mat[i].length`
-   `2 <= n, m <= 100`
-   `1 <= k <= m`
-   `matrix[i][j]` is either 0 or 1.

---
```java
// Java 2ms (Beats 80.96%), Time O(NlogN), Space O(N)
class Solution {
    public int[] kWeakestRows(int[][] mat, int k) {
        int[] ret = new int[k];
        int m = mat.length;
        int n = mat[0].length;
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a, b) -> {
            if (a[1] == b[1])
                return a[0] - b[0];
            return a[1] - b[1];
        });

        for (int i = 0; i < m; i++)
        {
            int soldiers = 0;
            for (int s : mat[i])
                soldiers += s;
            pq.add(new int[] {i, soldiers});
        }

        for (int i = 0; i < k; i++)
            ret[i] = pq.poll()[0];

        return ret;
    }
}
```
```csharp
// C# 121ms (Beats 100%), Time O(NlogN), Space O(N)
public class Solution {
    public int[] KWeakestRows(int[][] mat, int k) {
        int[] ret = new int[k];
        int m = mat.Length;
        int n = mat[0].Length;
        PriorityQueue<int, int[]> pq = new PriorityQueue<int, int[]>(Comparer<int[]>.Create((a, b) => {
            if (a[1] == b[1])
                return a[0] - b[0];
            return a[1] - b[1];
        }));

        for (int i = 0; i < m; i++)
        {
            int soldiers = 0;
            foreach (int s in mat[i])
                soldiers += s;
            pq.Enqueue(i, new int[] {i, soldiers});
        }

        for (int i = 0; i < k; i++)
            ret[i] = pq.Dequeue();

        return ret;
    }
}
```
---

###### tags: `Leetcode` `Priority Queue`