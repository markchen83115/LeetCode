# Leetcode - 542. 01 Matrix (M)

[Leetcode](https://leetcode.com/problems/01-matrix/description/)

Given an `m x n` binary matrix `mat`, return _the distance of the nearest _`0`_ for each cell_.

The distance between two adjacent cells is `1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/24/01-1-grid.jpg)
```
Input: mat = [[0,0,0],[0,1,0],[0,0,0]]
Output: [[0,0,0],[0,1,0],[0,0,0]]
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/24/01-2-grid.jpg)
```
Input: mat = [[0,0,0],[0,1,0],[1,1,1]]
Output: [[0,0,0],[0,1,0],[1,2,1]]
```
**Constraints:**

-   `m == mat.length`
-   `n == mat[i].length`
-   `1 <= m, n <= 104`
-   `1 <= m * n <= 104`
-   `mat[i][j]` is either `0` or `1`.
-   There is at least one `0` in `mat`.

---

```java
// Java BFS (19ms)
class Solution {
    public int[][] updateMatrix(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        Queue<Integer> queue = new LinkedList<>();
        // find all zero
        for (int i = 0; i < m; i ++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 0) {
                    queue.offer(i * n + j);
                } else {
                    mat[i][j] = Integer.MAX_VALUE;
                }
            }
        }
        int[] dir = {0 ,1, 0, -1, 0};
        // BFS
        while (!queue.isEmpty()) {
            int index = queue.poll();
            // use 0 to go right/up/left/down
            for (int k = 0; k < dir.length - 1; k++) {
                int row = index / n + dir[k];
                int col = index % n + dir[k+1];
                if (row >= 0 && row < m && col >= 0 && col < n 
                    && mat[row][col] != 0 
                    && mat[row][col] > mat[index/n][index%n] + 1) 
		{
                    mat[row][col] = mat[index/n][index%n] + 1;  // update distance
                    queue.offer(row * n + col);
                }
            }
        }
        return mat;
    }
}
```

```java
// Java DP (5ms)
class Solution {
    public int[][] updateMatrix(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int INF = m + n;
        //left top -> right down
        for (int i = 0; i < m; i ++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] != 0) {
                    int top = INF, left = INF;
                    if (i - 1 >= 0) 
                        top = mat[i-1][j];
                    if (j - 1 >= 0) 
                        left = mat[i][j-1];
                    mat[i][j] = Math.min(top, left) + 1;
                }
            }
        }

        //right down -> left top
        for (int i = m - 1; i >= 0; i --) {
            for (int j = n - 1; j >= 0; j--) {
                if (mat[i][j] != 0) {
                    int down = INF, right = INF;
                    if (i + 1 < m) 
                        down = mat[i+1][j];
                    if (j + 1 < n) 
                        right = mat[i][j+1];
					
		    // carefull: compare to mat[i][j]
                    mat[i][j] = Math.min(mat[i][j], Math.min(down, right) + 1);
                }
            }
        }

        return mat;
    }
}
```

---

Refer to [hiepit](https://leetcode.com/hiepit/)
https://leetcode.com/problems/01-matrix/solutions/1369741/c-java-python-bfs-dp-solutions-with-picture-clean-concise-o-1-space/


* BFS 
1. find all zero.
2. use BFS to get distance from zero.
![](https://i.imgur.com/JQAmg00.png)


* DP
1. from top/left to right/botton
2. from right/botton to top/left
![](https://i.imgur.com/U5EqQgS.png)



###### tags: `Leetcode` `BFS` `Dynamic Programming`
