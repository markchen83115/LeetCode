# Leetcode - 212. Word Search II (H)

[Leetcode](https://leetcode.com/problems/word-search-ii/description/)

Given an `m x n` `board` of characters and a list of strings `words`, return _all words on the board_.

Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)
```
Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)
```
Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []
```
**Constraints:**

-   `m == board.length`
-   `n == board[i].length`
-   `1 <= m, n <= 12`
-   `board[i][j]` is a lowercase English letter.
-   `1 <= words.length <= 3 * 104`
-   `1 <= words[i].length <= 10`
-   `words[i]` consists of lowercase English letters.
-   All the strings of `words` are unique.

---

```java
// Java 2124ms
class Solution {
    class TrieNode {
        TrieNode[] next = new TrieNode[26];
        boolean isEnd;
        TrieNode() {
            for (int i = 0; i < 26; i++)
                next[i] = null;
            isEnd = false;
        }
    }

    TrieNode root = new TrieNode();
    boolean[][] isVisited = new boolean[12][12];
    HashSet<String> set = new HashSet<>();
    int[] dir = new int[] {0, 1, 0, -1, 0};

    public List<String> findWords(char[][] board, String[] words) {
        // build trie by words
        for (String word : words) {
            TrieNode node = root;
            for (char c : word.toCharArray()) {
                if (node.next[c - 'a'] == null)
                    node.next[c - 'a'] = new TrieNode();
                node = node.next[c - 'a'];
            }
            node.isEnd = true;
        }

        // dfs
        int m = board.length, n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                dfs(board, i, j, root, "");
            }
        }

        List<String> ret = new ArrayList<>(set);
        return ret;
    }

    public void dfs(char[][] board, int i, int j, TrieNode node, String word) {
        int m = board.length, n = board[0].length;
        if (node.next[board[i][j] - 'a'] == null) return;
        node = node.next[board[i][j] - 'a'];
        isVisited[i][j] = true;
        word += board[i][j];
        
        if (node.isEnd)
            set.add(word);
        
        for (int k = 0; k < 4; k++) {
            int x = i + dir[k];
            int y = j + dir[k + 1];
            if (x < 0 || y < 0 || x >= m || y >= n || isVisited[x][y]) continue;
            dfs(board, x, y, node, word);
        }

        word.substring(0, word.length() - 1);
        isVisited[i][j] = false;
    }
}
```

```java
// Java 84ms
class Solution {
    class TrieNode {
        TrieNode[] next = new TrieNode[26];
        boolean isEnd;
        int count;
        TrieNode() {
            for (int i = 0; i < 26; i++)
                next[i] = null;
            isEnd = false;
            count = 0;
        }
    }

    TrieNode root = new TrieNode();
    boolean[][] isVisited = new boolean[12][12];
    List<String> ret = new ArrayList<>();
    int[] dir = new int[] {0, 1, 0, -1, 0};

    public List<String> findWords(char[][] board, String[] words) {
        // build trie by words
        for (String word : words) {
            TrieNode node = root;
            for (char c : word.toCharArray()) {
                if (node.next[c - 'a'] == null)
                    node.next[c - 'a'] = new TrieNode();
                node = node.next[c - 'a'];
                node.count += 1;
            }
            node.isEnd = true;
        }

        // dfs
        int m = board.length, n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                dfs(board, i, j, root, "");
            }
        }

        return ret;
    }

    public void dfs(char[][] board, int i, int j, TrieNode node, String word) {
        int m = board.length, n = board[0].length;
        if (node.next[board[i][j] - 'a'] == null) return;
        node = node.next[board[i][j] - 'a'];
        if (node.count <= 0) return;
        isVisited[i][j] = true;
        word += board[i][j];
        
        if (node.isEnd) {
            ret.add(word);
            remove(word);
        }
        
        for (int k = 0; k < 4; k++) {
            int x = i + dir[k];
            int y = j + dir[k + 1];
            if (x < 0 || y < 0 || x >= m || y >= n || isVisited[x][y]) continue;
            dfs(board, x, y, node, word);
        }

        word.substring(0, word.length() - 1);
        isVisited[i][j] = false;
    }

    public void remove(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            node = node.next[c - 'a'];
            node.count -= 1;
        }
        node.isEnd = false;
    }
}
```

---
[wisdompeak/YouTube](https://www.youtube.com/watch?v=nGGF_jQH0GQ)

將要尋找的words建成Trie, dfs+Trie進行搜尋

優化:
因為dfs時會一直重複搜尋到相同的word, 
因此將已搜尋過的word從Trie中移除, 優化在Trie中的搜尋次數
因為從Trie移除word相當麻煩,
例如: `[abc], [abc]ab, [abc]eaa` 有相同的開頭, 
當搜尋到`abc`時, 不可將abc移除, 否則後續會斷頭

方法:
在TrieNode加上`count`屬性, 
當搜尋到字詞時, 將其所有字元的`count - 1`並且`isEnd = false`避免重複減少`count`
當後續搜尋字詞時, 若`count = 0`代表之後已無任何字詞, 則不需繼續往下搜尋

###### tags: `Leetcode` `Trie`
