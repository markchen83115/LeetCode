# Leetcode - 127. Word Ladder (H)

[Leetcode](https://leetcode.com/problems/word-ladder/description/)

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

-   Every adjacent pair of words differs by a single letter.
-   Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
-   `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _the **number of words** in the **shortest transformation sequence** from_ `beginWord` _to_ `endWord`_, or _`0`_ if no such sequence exists._

**Example 1:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.
```
**Example 2:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
```
**Constraints:**

-   `1 <= beginWord.length <= 10`
-   `endWord.length == beginWord.length`
-   `1 <= wordList.length <= 5000`
-   `wordList[i].length == beginWord.length`
-   `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
-   `beginWord != endWord`
-   All the words in `wordList` are **unique**.

---
```java
// Java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashSet<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord))
            return 0;
        Queue<String> queue = new LinkedList<>();
        HashSet<String> isVisited = new HashSet<>();
        queue.offer(beginWord);
        isVisited.add(beginWord);

        int counts = 1;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String str = queue.poll();
                if (str.equals(endWord))
                    return counts;
                
                // [b]e -> [c]e
                char[] arr = str.toCharArray();
                for (int j = 0; j < str.length(); j++) {
                    for (int k = 'a'; k <= 'z'; k++) {
                        char orgChar = arr[j];
                        arr[j] = (char) k;
                        String nextStr = new String(arr);
                        if (wordSet.contains(nextStr) && !isVisited.contains(nextStr)) {
                            queue.offer(nextStr);
                            isVisited.add(nextStr);
                        }
                        arr[j] = orgChar;
                    }
                }
            }
            counts++;
        }
        return 0;
    }
}
```

---

以下參考至Leetcode [@hi-malik](https://leetcode.com/hi-malik/) [詳細解法](https://leetcode.com/problems/word-ladder/solutions/1764371/a-very-highly-detailed-explanation/)

```a
start = be
end = ko
words = ["ce", "mo", "ko", "me", "co"]
```

```a
queue = ["be"  ]
changes = 1
set = ["be"  ]
```

![](https://i.imgur.com/Nvr4Rfr.png)

![](https://i.imgur.com/1HmYh3n.png)

![](https://i.imgur.com/jUs1Gjg.png)

![](https://i.imgur.com/J5ooiVU.png)

![](https://i.imgur.com/uKbQ76j.png)

![](https://i.imgur.com/b8B9PB7.png)

```a
queue = [  ]
changes = 4
set = ["be", "ce", "me", "co", "mo", "ko"  ]
```


###### tags: `Leetcode` `BFS`
