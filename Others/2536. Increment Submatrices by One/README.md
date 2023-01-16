# Leetcode - 2536. Increment Submatrices by One (H-)

[Leetcode](https://leetcode.com/problems/increment-submatrices-by-one/description/)

You are given a positive integer `n`, indicating that we initially have an `n x n` **0-indexed** integer matrix `mat` filled with zeroes.

You are also given a 2D integer array `query`. For each `query[i] = [row1i, col1i, row2i, col2i]`, you should do the following operation:

-   Add `1` to **every element** in the submatrix with the **top left** corner `(row1i, col1i)` and the **bottom right** corner `(row2i, col2i)`. That is, add `1` to `mat[x][y]` for for all `row1i <= x <= row2i` and `col1i <= y <= col2i`.

Return_ the matrix_ `mat`_ after performing every query._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/11/24/p2example11.png)
```
Input: n = 3, queries = [[1,1,2,2],[0,0,1,1]]
Output: [[1,1,0],[1,2,1],[0,1,1]]
Explanation: The diagram above shows the initial matrix, the matrix after the first query, and the matrix after the second query.
- In the first query, we add 1 to every element in the submatrix with the top left corner (1, 1) and bottom right corner (2, 2).
- In the second query, we add 1 to every element in the submatrix with the top left corner (0, 0) and bottom right corner (1, 1).
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2022/11/24/p2example22.png)
```
Input: n = 2, queries = [[0,0,1,1]]
Output: [[1,1],[1,1]]
Explanation: The diagram above shows the initial matrix and the matrix after the first query.
- In the first query we add 1 to every element in the matrix.
```
**Constraints:**

-   `1 <= n <= 500`
-   `1 <= queries.length <= 104`
-   `0 <= row1i <= row2i < n`
-   `0 <= col1i <= col2i < n`

---

```java
// Java TC: O(n^2) SC: O(3 * n^2)
class Solution {
    public int[][] rangeAddQueries(int n, int[][] queries) {
        int[][] diff = new int[n+1][n+1];
        int[][] f = new int[n+1][n+1];
        int[][] mat = new int[n][n];
        for (int[] query : queries) {
            int staRow = query[0];
            int staCol = query[1];
            int endRow = query[2];
            int endCol = query[3];
            diff[staRow][staCol] += 1;
            diff[staRow][endCol + 1] -= 1;
            diff[endRow + 1][staCol] -= 1;
            diff[endRow + 1][endCol + 1] += 1;
        }

        f[0][0] = diff[0][0];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int a = i == 0 ? 0 : f[i - 1][j];
                int b = j == 0 ? 0 : f[i][j - 1];
                int c = (i == 0 || j == 0) ? 0 : f[i - 1][j - 1];
                f[i][j] = a + b - c + diff[i][j];
				mat[i][j] = f[i][j];
            }
        }

        return mat;
    }
}
```

```java
// Java TC: O(2 * n^2) SC: O(2 * n^2)
class Solution {
    public int[][] rangeAddQueries(int n, int[][] queries) {
        int[][] diff = new int[n+1][n+1];
        int[][] mat = new int[n][n];
        for (int[] query : queries) {
            int staRow = query[0];
            int staCol = query[1];
            int endRow = query[2];
            int endCol = query[3];
            diff[staRow][staCol] += 1;
            diff[staRow][endCol + 1] -= 1;
            diff[endRow + 1][staCol] -= 1;
            diff[endRow + 1][endCol + 1] += 1;
        }

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                diff[i + 1][j] += diff[i][j];

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++) {
                diff[i][j + 1] += diff[i][j];
                mat[i][j] = diff[i][j];
            }

        return mat;
    }
}
```


---
參考：
[wisdompeak/Youtube解說](https://www.youtube.com/watch?v=J2TUaneNk90&list=PLwdV8xC1EWHrtgsYCcDTXIMVaHSlsnLzL&index=1)
[wisdompeak/二維差分模板](https://github.com/wisdompeak/LeetCode/tree/master/Template/Diff_Array_2D)

```cpp
// C++
class Diff2d {    
    public:
        vector<vector<int>>f;
        vector<vector<int>>diff;    
        int m,n;
        Diff2d(int m, int n)
        {
            this->m = m;
            this->n = n;
            diff.resize(m+1);
            f.resize(m+1);        
            for (int i=0; i<m+1; i++)
            {
                diff[i].resize(n+1);
                f[i].resize(n+1);
            }            
        }
        void set(int x0, int y0, int x1, int y1, int val)
        {
            diff[x0][y0]+=val;
            diff[x0][y1+1]-=val;
            diff[x1+1][y0]-=val;
            diff[x1+1][y1+1]+=val;
        }
        void compute()
        {
            f[0][0] = diff[0][0];
            for (int i=0; i<m; i++)
                for (int j=0; j<n; j++)
                {
                    int a = i==0?0:f[i-1][j];
                    int b = j==0?0:f[i][j-1];
                    int c = (i==0||j==0)?0:f[i-1][j-1];                
                    f[i][j] = a + b - c + diff[i][j];
                }
        }    
    };
```

###### tags: `Leetcode` `二維差分`