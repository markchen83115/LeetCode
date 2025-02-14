# Leetcode - 068. Text Justification (H)

[Leetcode](https://leetcode.com/problems/text-justification/)

Given an array of strings `words` and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly `maxWidth` characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

**Note:**

-   A word is defined as a character sequence consisting of non-space characters only.
-   Each word's length is guaranteed to be greater than `0` and not exceed `maxWidth`.
-   The input array `words` contains at least one word.

**Example 1:**

> **Input:** words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
> **Output:**
> [
>    "This    is    an",
>    "example  of text",
>    "justification.  "
> ]

**Example 2:**

> **Input:** words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
> **Output:**
> [
>   "What   must   be",
>   "acknowledgment  ",
>   "shall be        "
> ]
> **Explanation:** Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
> Note that the second line is also left-justified because it contains only one word.

**Example 3:**

> **Input:** words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
> **Output:**
> [
>   "Science  is  what we",
>   "understand      well",
>   "enough to explain to",
>   "a  computer.  Art is",
>   "everything  else  we",
>   "do                  "
> ]

**Constraints:**

-   `1 <= words.length <= 300`
-   `1 <= words[i].length <= 20`
-   `words[i]` consists of only English letters and symbols.
-   `1 <= maxWidth <= 100`
-   `words[i].length <= maxWidth`

---
```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> ret = new ArrayList<>();
        HashMap<Integer, List<String>> group = new HashMap<>();
        HashMap<Integer, Integer> groupLen = new HashMap<>();
        int g = 1;
        groupLen.put(g, 0);
        group.put(g, new ArrayList<>());

        for (String w : words)
        {
            if (groupLen.get(g) + group.get(g).size() + w.length() > maxWidth)
                g++;

            if (!groupLen.containsKey(g))
            {
                groupLen.put(g, 0);
                group.put(g, new ArrayList<>());
            }

            group.get(g).add(w);
            groupLen.merge(g, w.length(), Integer::sum);
        }

        for (int i = 1; i <= g; i++)
        {
            StringBuilder sb = new StringBuilder();
            List<String> list = group.get(i);
            int len = groupLen.get(i);
            if (i == g)
            {
                for (int j = 0; j < list.size(); j++)
                    sb.append(list.get(j) + " ");
                sb.delete(sb.length() - 1, sb.length());
                while (sb.length() < maxWidth)
                    sb.append(" ");
            }
            else
            {
                int j = 0;
                int spaceLen = (maxWidth - len) / Math.max(1, list.size() - 1);
                int spaceExtra = (maxWidth - len) % Math.max(1, list.size() - 1);
                for (int k = 0; k < spaceExtra; k++)
                {
                    sb.append(list.get(j));
                    for (int s = 1; s <= spaceLen + 1; s++)
                        sb.append(" ");
                    j++;
                }
                for (; j < list.size(); j++)
                {
                    sb.append(list.get(j));
                    for (int s = 1; s <= spaceLen && j < list.size() - 1; s++)
                        sb.append(" ");
                }
                while (sb.length() < maxWidth)
                    sb.append(" ");
            }

            ret.add(sb.toString());
        }

        return ret;
    }
}
```
---

這是一道比較複雜的模擬題，需要耐心地做。

首先我們要做的是找出每一行需要印多少單字。假設某行開始，我們需要從words[i]開始列印直到到words[j]為止，確定這個[i:j]的區間需要滿足的條件是：words[i:j]的字元長度加上(j-i)個空格，必須恰好小於等於maxWidth。然後我們就可以呼叫一個自訂的`printLine`的函數來控制如何在maxWidth裡面印出這麼多單字。唯一例外的是，如果發現j是最後一個單詞，那麼需要特殊處理一下，空格就不需要在單字之間平均分佈。

接下來我們寫函數`string printLine(i,j)`. 我們需要先計算空格總數`m = maxWidth - words[i:j]的長度和`，試圖均分給(j-i)個間隙，可以得到基礎間隙大小是`s = m/(j-i)`. 此時我們可能存個空格後需要留個空格。

```java
for (int i = a; i < a + k; i++)
   ret += words[i] + addspace(sc+c1);
for (int i = a + k; i < b; i++)
   ret += words[i] + addspace(s);
ret += words[b];
```


###### tags: `Leetcode` `String`