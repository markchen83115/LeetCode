# Leetcode - 1233. Remove Sub-Folders from the Filesystem (M)

[Leetcode](https://leetcode.com/problems/remove-sub-folders-from-the-filesystem/)

Given a list of folders `folder`, return _the folders after removing all **sub-folders** in those folders_. You may return the answer in **any order**.

If a `folder[i]` is located within another `folder[j]`, it is called a **sub-folder** of it. A sub-folder of `folder[j]` must start with `folder[j]`, followed by a `"/"`. For example, `"/a/b"` is a sub-folder of `"/a"`, but `"/b"` is not a sub-folder of `"/a/b/c"`.

The format of a path is one or more concatenated strings of the form: `'/'` followed by one or more lowercase English letters.

-   For example, `"/leetcode"` and `"/leetcode/problems"` are valid paths while an empty string and `"/"` are not.

**Example 1:**
```
Input: folder = ["/a","/a/b","/c/d","/c/d/e","/c/f"]
Output: ["/a","/c/d","/c/f"]
Explanation: Folders "/a/b" is a subfolder of "/a" and "/c/d/e" is inside of folder "/c/d" in our filesystem.
```
**Example 2:**
```
Input: folder = ["/a","/a/b/c","/a/b/d"]
Output: ["/a"]
Explanation: Folders "/a/b/c" and "/a/b/d" will be removed because they are subfolders of "/a".
```
**Example 3:**
```
Input: folder = ["/a/b/c","/a/b/ca","/a/b/d"]
Output: ["/a/b/c","/a/b/ca","/a/b/d"]
```
**Constraints:**

-   `1 <= folder.length <= 4 * 104`
-   `2 <= folder[i].length <= 100`
-   `folder[i]` contains only lowercase letters and `'/'`.
-   `folder[i]` always starts with the character `'/'`.
-   Each folder name is **unique**.

---
```java
// Java Sort 42ms(Beats 75.31%), Time O(NlogN), Space O(N)
class Solution {
    public List<String> removeSubfolders(String[] folder) {
        List<String> ret = new ArrayList<>();
        Arrays.sort(folder);
        ret.add(folder[0]);
        for (int i = 1; i < folder.length; i++)
        {
            String prev = ret.get(ret.size() - 1);
            String curr = folder[i];
            if (prev.charAt(1) != curr.charAt(1))
            {
                ret.add(curr);
                continue;
            }

            int p = 0;
            while (p < prev.length() && prev.charAt(p) == curr.charAt(p))
                p++;
            
            if (p == prev.length() && curr.charAt(p) == '/')
                continue;
            
            ret.add(curr);
        }
        return ret;
    }
}
```
```java
// Java Trie 72ms(Beats 31.43%), Time O(NlogN), Space O(N)
class Solution {
    class Trie {
        HashMap<String, Trie> next;
        boolean isEnd;

        Trie() {
            next = new HashMap<>();
            isEnd = false;
        }
    }
    public List<String> removeSubfolders(String[] folder) {
        List<String> ret = new ArrayList<>();
        Arrays.sort(folder, (a, b) -> a.length() - b.length());
        Trie root = new Trie();
        // build Trie
        for (String f : folder)
        {
            Trie node = root;
            for (String s : f.split("/"))
            {
                if (node.isEnd)
                    break;
                if (!node.next.containsKey(s))
                    node.next.put(s, new Trie());

                node = node.next.get(s);
            }
            node.isEnd = true;
        }

        // traversal Trie
        for (String f : folder)
        {
            Trie node = root;
            String[] splitArr = f.split("/");
            for (int i = 0; i < splitArr.length; i++)
            {
                node = node.next.get(splitArr[i]);

                if (i == splitArr.length - 1)
                    ret.add(f);
                if (node.isEnd)
                    break;
            }
            
        }

        return ret;
    }
}
```
---

可利用`Sting[]`排序之後的結果
`["/c/d/e", "/c/f", "/a/b", "/c/d", "/a"]`
排序後為`["/a", "/a/b", "/c/d", "/c/d/e", "/c/f"]`
因此可快速從第一個開始往後檢查, 是否`後一個(i-1)`為`前一個(i)`的`sub-folders`



###### tags: `Leetcode`	`Trie`