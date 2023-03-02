# Leetcode - 79. Word Search (M)

[Leetcode](https://leetcode.com/problems/word-search/description/)

Given an `m x n` grid of characters `board` and a string `word`, return `true` _if_ `word` _exists in the grid_.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
```
**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```
**Constraints:**

-   `m == board.length`
-   `n = board[i].length`
-   `1 <= m, n <= 6`
-   `1 <= word.length <= 15`
-   `board` and `word` consists of only lowercase and uppercase English letters.

**Follow up:** Could you use search pruning to make your solution faster with a larger `board`?

---
```java
// Java
class Solution {
    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        char[] arr = word.toCharArray();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == arr[0]) {
                    if (dfs(board, arr, i, j, 0)) return true;
                }
            }
        }
        return false;
    }

    public boolean dfs(char[][] board, char[] arr, int i, int j, int k) {
        int m = board.length;
        int n = board[0].length;
        if (k == arr.length) return true;
        if (i < 0 || j < 0 || i >= m || j >= n || k >= arr.length) return false;
        if (board[i][j] != arr[k]) return false;
        board[i][j] = '*';
        if (dfs(board, arr, i + 1, j, k + 1) ||
            dfs(board, arr, i - 1, j, k + 1) ||
            dfs(board, arr, i, j + 1, k + 1) ||
            dfs(board, arr, i, j - 1, k + 1)) {
            return true;
        }
        board[i][j] = arr[k];
        return false;
    }
}
```

---



###### tags: `Leetcode` `DFS`
