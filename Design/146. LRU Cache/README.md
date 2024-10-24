# Leetcode - 146. LRU Cache (H-)

[Leetcode](https://leetcode.com/problems/lru-cache/description/)

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

-   `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
-   `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
-   `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**
```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```
**Constraints:**

-   `1 <= capacity <= 3000`
-   `0 <= key <= 104`
-   `0 <= value <= 105`
-   At most `2 * 105` calls will be made to `get` and `put`.

---

```java
// Java 43ms(Beats 86.35%), Time O(1), Space O(N)
class LRUCache {
    class ListNode {
        int key;
        int val;
        ListNode prev;
        ListNode next;
    }

    int n = 0;
    ListNode head, tail;
    HashMap<Integer, ListNode> key2Node;
    public LRUCache(int capacity) {
        this.n = capacity;
        head = new ListNode();
        tail = new ListNode();
        head.next = tail;
        tail.prev = head;
        key2Node = new HashMap<>();
    }
    // head x x x x x tail
    //          k
    public int get(int key) {
        if (!key2Node.containsKey(key))
            return -1;
        ListNode k = key2Node.get(key);
        updateCache(k);

        return k.val;
    }
    
    public void put(int key, int value) {
        if (key2Node.containsKey(key))
        {
            ListNode k = key2Node.get(key);
            k.val = value;
            updateCache(k);
            return;
        }

        // evict head
        if (key2Node.size() == n)
        {
            ListNode k = head.next;
            key2Node.remove(k.key);
            k.next.prev = head;
            head.next = k.next;
        }

        ListNode k = new ListNode();
        k.key = key;
        k.val = value;
        key2Node.put(key, k);
        updateCache(k);
    }

    void updateCache(ListNode k)
    {
        //remove k
        ListNode pre = k.prev;
        ListNode nxt = k.next;
        if (pre != null)
            pre.next = nxt;
        if (nxt != null)
            nxt.prev = pre;
        // put k to tail
        pre = tail.prev;
        pre.next = k;
        k.next = tail;
        k.prev = pre;
        tail.prev = k;
    }
}
/*
evict head
update -> put last
[x x x x x x x]
*/
```
---

利用double linked list
寫出remove()移除node的function以及將node加到Head的function

###### tags: `Leetcode` `Design` `Linked List`