# Leetcode - 51. N-Queens (M)

[Leetcode](https://leetcode.com/problems/n-queens/description/)

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)
```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
```
**Example 2:**
```
Input: n = 1
Output: [["Q"]]
```
**Constraints:**

-   `1 <= n <= 9`

---
```java
// Java
class Solution {
    List<List<String>> ret = new ArrayList<>();
    public List<List<String>> solveNQueens(int n) {
        char[][] mat = new char[n][n];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                mat[i][j] = '.';
        backtracking(n, 0, mat);
        return ret;
    }

    public void backtracking(int n, int row, char[][] mat) {
        if (row == n) {
            List<String> l = new ArrayList<>();
            for (char[] charArr : mat) {
                l.add(String.valueOf(charArr));
            }
            ret.add(l);
            return;
        }
        
        for (int col = 0; col < n; col++) {
            if (isOK(n, row, col, mat)) {
                mat[row][col] = 'Q';
                backtracking(n, row + 1, mat);
                mat[row][col] = '.';
            }
        }
    }

    public boolean isOK(int n, int row, int col, char[][] mat) {
        //upper
        for (int i = row - 1; i >= 0; i--) {
            if (mat[i][col] == 'Q') 
                return false;
        }
        // upper left 
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (mat[i][j] == 'Q') 
                return false;
        }
        // upper right
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (mat[i][j] == 'Q') 
                return false;
        }
        return true;
    }
}
```

```csharp
// C#
class Solution {
    List<IList<string>> ret = new List<IList<string>>();
    public IList<IList<string>> SolveNQueens(int n) {
        char[][] mat = new char[n][];
        for (int i = 0; i < n; i++) {
            char[] inner = new char[n];
            for (int j = 0; j < n; j++) {
                inner[j] = '.';
            }
            mat[i] = inner;
        }
        backtracking(n, 0, mat);
        return ret;
    }

    public void backtracking(int n, int row, char[][] mat) {
        if (row == n) {
            List<string> l = new List<string>();
            for (int i = 0; i < mat.Length; i++) {
                l.Add(new string(mat[i]));
            }
            ret.Add(l);
            return;
        }
        
        for (int col = 0; col < n; col++) {
            if (isOK(n, row, col, mat)) {
                mat[row][col] = 'Q';
                backtracking(n, row + 1, mat);
                mat[row][col] = '.';
            }
        }
    }

    public bool isOK(int n, int row, int col, char[][] mat) {
        //upper
        for (int i = row - 1; i >= 0; i--) {
            if (mat[i][col] == 'Q') 
                return false;
        }
        // upper left 
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (mat[i][j] == 'Q') 
                return false;
        }
        // upper right
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (mat[i][j] == 'Q') 
                return false;
        }
        return true;
    }
}
```

---


###### tags: `Leetcode` `DFS`
