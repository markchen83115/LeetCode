# Leetcode - 208. Implement Trie (Prefix Tree) (M+)

[Leetcode](https://leetcode.com/problems/implement-trie-prefix-tree/description/)

A [**trie**](https://en.wikipedia.org/wiki/Trie) (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

-   `Trie()` Initializes the trie object.
-   `void insert(String word)` Inserts the string `word` into the trie.
-   `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
-   `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

**Example 1:**
```
Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
```
**Constraints:**

-   `1 <= word.length, prefix.length <= 2000`
-   `word` and `prefix` consist only of lowercase English letters.
-   At most `3 * 104` calls **in total** will be made to `insert`, `search`, and `startsWith`.

---
```java
// Java 35ms(Beats 99.62%)
class Trie {
    Node root;

    public Trie() {
        root = new Node();
    }
    
    public void insert(String word) {
        Node node = root;
        for (char c : word.toCharArray()) {
            if (node.next[c - 'a'] == null)
                node.next[c - 'a'] = new Node();
            node = node.next[c - 'a'];
        }
        node.isEnd = true;
    }
    
    public boolean search(String word) {
        Node node = root;
        for (char c : word.toCharArray()) {
            if (node.next[c - 'a'] == null)
                return false;
            node = node.next[c - 'a'];
        }
        return node.isEnd;
    }
    
    public boolean startsWith(String prefix) {
        Node node = root;
        for (char c : prefix.toCharArray()) {
            if (node.next[c - 'a'] == null)
                return false;
            node = node.next[c - 'a'];
        }
        return true;
    }
}

public class Node {
    public boolean isEnd;
    public Node[] next;
    
    public Node() {
        next = new Node[26];
    }
}
```

```csharp
// C# 245ms(Beats 86.20%)
public class Trie {
    Node root;

    public Trie() {
        root = new Node();    
    }
    
    public void Insert(string word) {
        Node node = root;
        foreach (char c in word) {
            if (node.next[c - 'a'] == null)
                node.next[c - 'a'] = new Node();
            node = node.next[c - 'a'];
        }
        node.isEnd = true;
    }
    
    public bool Search(string word) {
        Node node = root;
        foreach (char c in word) {
            if (node.next[c - 'a'] == null)
                return false;
            node = node.next[c - 'a'];
        }
        return node.isEnd;
    }
    
    public bool StartsWith(string prefix) {
        Node node = root;
        foreach (char c in prefix) {
            if (node.next[c - 'a'] == null)
                return false;
            node = node.next[c - 'a'];
        }
        return true;
    }
}

public class Node {
    public bool isEnd;
    public Node[] next;

    public Node() {
        next = new Node[26];
    }
}
```


---


###### tags: `Leetcode` `Trie`