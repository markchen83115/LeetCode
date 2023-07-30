# Leetcode - 211. Design Add and Search Words Data Structure (H-)

[Leetcode](https://leetcode.com/problems/design-add-and-search-words-data-structure/description/)

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:

-   `WordDictionary()` Initializes the object.
-   `void addWord(word)` Adds `word` to the data structure, it can be matched later.
-   `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

**Example:**
```
Input
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
Output
[null,null,null,null,false,true,true,true]

Explanation
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True
```
**Constraints:**

-   `1 <= word.length <= 25`
-   `word` in `addWord` consists of lowercase English letters.
-   `word` in `search` consist of `'.'` or lowercase English letters.
-   There will be at most `2` dots in `word` for `search` queries.
-   At most `104` calls will be made to `addWord` and `search`.

---
```java
// Java 239ms (Beats 53.97%), 
public class WordDictionary {
    WordDictionary[] next;
    boolean isEnd;

    public WordDictionary() {
        next = new WordDictionary[26];
        isEnd = false;
    }
    
    public void addWord(String word) {
        WordDictionary curr = this;
        for (char c : word.toCharArray()) {
            if (curr.next[c - 'a'] == null)
                curr.next[c - 'a'] = new WordDictionary();
            curr = curr.next[c - 'a'];
        }
        curr.isEnd = true;
    }
    
    public boolean search(String word) {
        WordDictionary curr = this;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (c == '.') {
                for (WordDictionary wd : curr.next) {
                    if (wd != null && wd.search(word.substring(i + 1)))
                        return true;
                }
                return false;
            }
            if (curr.next[c - 'a'] == null)
                return false;

            curr = curr.next[c - 'a'];
        }
        return curr.isEnd;
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```

```csharp
// C# 812ms (Beats 97.69%)
public class WordDictionary {
    WordDictionary[] next;
    bool isEnd;

    public WordDictionary() {
        next = new WordDictionary[26];
        isEnd = false;
    }
    
    public void AddWord(string word) {
        WordDictionary curr = this;
        foreach (char c in word) {
            if (curr.next[c - 'a'] == null)
                curr.next[c - 'a'] = new WordDictionary();
            curr = curr.next[c - 'a'];
        }
        curr.isEnd = true;
    }
    
    public bool Search(string word) {
        WordDictionary curr = this;
        for (int i = 0; i < word.Length; i++) {
            char c = word[i];
            if (c == '.') {
                foreach (WordDictionary wd in curr.next) {
                    if (wd != null && wd.Search(word.Substring(i + 1)))
                        return true;
                }
                return false;
            }
            if (curr.next[c - 'a'] == null)
                return false;

            curr = curr.next[c - 'a'];
        }
        return curr.isEnd;
    }
}
```
---

###### tags: `Leetcode` `Trie`