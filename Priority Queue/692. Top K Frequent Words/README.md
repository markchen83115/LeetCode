# Leetcode - 692. Top K Frequent Words (M)

[Leetcode](https://leetcode.com/problems/top-k-frequent-words/description/)

Given an array of strings `words` and an integer `k`, return _the _`k`_ most frequent strings_.

Return the answer **sorted** by **the frequency** from highest to lowest. Sort the words with the same frequency by their **lexicographical order**.

**Example 1:**
```
Input: words = ["i","love","leetcode","i","love","coding"], k = 2
Output: ["i","love"]
Explanation: "i" and "love" are the two most frequent words.
Note that "i" comes before "love" due to a lower alphabetical order.
```
**Example 2:**
```
Input: words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4
Output: ["the","is","sunny","day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words, with the number of occurrence being 4, 3, 2 and 1 respectively.
```
**Constraints:**

-   `1 <= words.length <= 500`
-   `1 <= words[i].length <= 10`
-   `words[i]` consists of lowercase English letters.
-   `k` is in the range `[1, The number of **unique** words[i]]`

**Follow-up:** Could you solve it in `O(n log(k))` time and `O(n)` extra space?

---
```java
// Java [Sort] 6ms (Beats 86.20%), Time O(NlogN), Space O(N)
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> freq = new HashMap<>();
        for (String word : words)
            freq.merge(word, 1, Integer::sum);

        List<String> ret = new ArrayList(freq.keySet());
        Collections.sort(ret, (a, b) -> {
            if (freq.get(a) == freq.get(b))
                return a.compareTo(b);
            return freq.get(b) - freq.get(a);
        });

        return ret.subList(0, k);
    }
}
```

```java
// Java [Min Heap] 6ms (Beats 86.20%), Time O(NlogK), Space O(N)
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> freq = new HashMap<>();
        for (String word : words)
            freq.merge(word, 1, Integer::sum);

        PriorityQueue<Map.Entry<String, Integer>> pq = new PriorityQueue<>((a, b) -> {
            if (a.getValue() == b.getValue())
                return b.getKey().compareTo(a.getKey());
            return a.getValue() - b.getValue();
        });
        
        for (Map.Entry<String, Integer> kv : freq.entrySet()) {
            pq.offer(kv);
            if (pq.size() > k)
                pq.poll();
        }

        List<String> ret = new LinkedList<>();
        for (int i = 0; i < k; i++)
            ret.add(0, pq.poll().getKey());

        return ret;
    }
}
```
---


###### tags: `Leetcode` `Priority Queue`