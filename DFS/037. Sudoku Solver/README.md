# Leetcode - 37. Sudoku Solver (M+)

[Leetcode](https://leetcode.com/problems/sudoku-solver/)

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1.  Each of the digits `1-9` must occur exactly once in each row.
2.  Each of the digits `1-9` must occur exactly once in each column.
3.  Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

The `'.'` character indicates empty cells.

**Example 1:**

![](https://miro.medium.com/max/500/0*5eTQ7rxYaA6qxyiv.png)
```
Input: board = 
[["5","3",".",".","7",".",".",".","."],
["6",".",".","1","9","5",".",".","."],
[".","9","8",".",".",".",".","6","."],
["8",".",".",".","6",".",".",".","3"],
["4",".",".","8",".","3",".",".","1"],
["7",".",".",".","2",".",".",".","6"],
[".","6",".",".",".",".","2","8","."],
[".",".",".","4","1","9",".",".","5"],
[".",".",".",".","8",".",".","7","9"]]  

Output: 
[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],
["1","9","8","3","4","2","5","6","7"],
["8","5","9","7","6","1","4","2","3"],
["4","2","6","8","5","3","7","9","1"],
["7","1","3","9","2","4","8","5","6"],
["9","6","1","5","3","7","2","8","4"],
["2","8","7","4","1","9","6","3","5"],
["3","4","5","2","8","6","1","7","9"]] 

Explanation: The input board is shown above and the only valid solution is shown below:
```
![](https://miro.medium.com/max/500/0*cvch2y8kSvsSTKXo.png)

**Constraints:**

-   `board.length == 9`
-   `board[i].length == 9`
-   `board[i][j]` is a digit or `'.'`.
-   It is **guaranteed** that the input board has only one solution.

---

```java
// Java  
class Solution {  
    public void solveSudoku(char[][] board) {  
        dfs(board, 0, 0);  
    }  
  
    public boolean dfs(char[][] board, int i, int j) {  
        if (i == 9)  
            return true;  
        if (j == 9)  
            return dfs(board, i + 1, 0);  
        if (board[i][j] != '.')  
            return dfs(board, i, j + 1);  
  
        for (char k = '1'; k <= '9'; k++) {  
            if (!isValid(board, i, j, k))   
                continue;  
  
            board[i][j] = k;  
            if (dfs(board, i, j + 1) == true) {  
                return true;  
            }  
            board[i][j] = '.';  
        }  
  
        return false;  
    }  
  
    public boolean isValid(char[][] board, int i, int j, char k) {  
        for (int p = 0; p < 9; p++) {  
            if (board[p][j] == k)   
                return false;  
            if (board[i][p] == k)  
                return false;  
            int r = i / 3 * 3 + p / 3;  
            int c = j / 3 * 3 + p % 3;  
            if (board[r][c] == k)  
                return false;  
        }  
        return true;  
    }  
}
```

---


用DFS遍歷所有格子, 嘗試放入1–9的數字

若此格子`[i, j]`已用完1–9的數字皆不行,  
則退回格子`[i, j-1]`嘗試下一個數字


###### tags: `Leetcode` `DFS`
