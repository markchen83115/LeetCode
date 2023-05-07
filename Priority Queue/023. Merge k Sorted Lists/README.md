# Leetcode - 23. Merge k Sorted Lists (M)

[Leetcode](https://leetcode.com/problems/merge-k-sorted-lists/description/)

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**
```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```
**Example 2:**
```
Input: lists = []
Output: []
```
**Example 3:**
```
Input: lists = [[]]
Output: []
```
**Constraints:**

-   `k == lists.length`
-   `0 <= k <= 104`
-   `0 <= lists[i].length <= 500`
-   `-104 <= lists[i][j] <= 104`
-   `lists[i]` is sorted in **ascending order**.
-   The sum of `lists[i].length` will not exceed `104`.

---
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
// Java 5ms (Beats 69.57%), Time O(N), Space O(N)
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode head = new ListNode();
        ListNode dummy = head;
        PriorityQueue<ListNode> pq = new PriorityQueue<>((a , b) -> a.val - b.val);
        int n = lists.length;
        for (int i = 0; i < n; i++) {
            if (lists[i] != null)
                pq.offer(lists[i]);
        }

        while (!pq.isEmpty()) {
            ListNode node = pq.poll();
            head.next = node;
            head = head.next;
            if (node.next != null)
                pq.offer(node.next);
        }

        return dummy.next;
    }
}
```

---
###### tags: `Leetcode` `Priority Queue`