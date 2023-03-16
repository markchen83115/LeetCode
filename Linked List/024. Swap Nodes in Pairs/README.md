# Leetcode - 24. Swap Nodes in Pairs (M)

[Leetcode](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)
```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```
**Example 2:**
```
Input: head = []
Output: []
```
**Example 3:**
```
Input: head = [1]
Output: [1]
```
**Constraints:**

-   The number of nodes in the list is in the range `[0, 100]`.
-   `0 <= Node.val <= 100`

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
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode();
        dummy.next = head;
        ListNode first = dummy;
        while (first.next != null && first.next.next != null) {
            // 1 -> 2 -> 3 -> 4 => 1 -> 3 -> 2 -> 4
            ListNode sec = first.next;
            first.next = sec.next;  // 1 -> 3
            sec.next = first.next.next; // 2 -> 4
            first.next.next = sec;   // 3 -> 2 
            first = first.next.next;
        }
        return dummy.next;
    }
}
```


---

###### tags: `Leetcode` `Linked List`
