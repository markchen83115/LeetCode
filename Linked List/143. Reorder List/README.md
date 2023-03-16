# Leetcode - 143. Reorder List (H-)

[Leetcode](https://leetcode.com/problems/reorder-list/description/)

You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln

_Reorder the list to be on the following form:_

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)
```
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)
```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```
**Constraints:**

-   The number of nodes in the list is in the range `[1, 5 * 104]`.
-   `1 <= Node.val <= 1000`

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
    public void reorderList(ListNode head) {
        ListNode fast = head, slow = head;
        // find mid
        // 1 -> 2  -> 3  -> 4  -> 5
        //            s           f
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode prev = slow;
        slow = slow.next;
        prev.next = null;

        // reverse from mid
        // 1 -> 2  -> 3    null <- 4 <- 5
        prev = null;
        while (slow != null) {
            ListNode next = slow.next;
            slow.next = prev;
            prev = slow;
            slow = next;
        }

        // 1 -> 5 -> 2 -> 4 -> 3
        slow = prev;
        while (slow != null) {
            ListNode headNext = head.next;
            ListNode slowNext = slow.next;
            head.next = slow;
            slow.next = headNext;
            slow = slowNext;
            head = headNext;
        }
    }
}
```


---

###### tags: `Leetcode` `Linked List` `Reverse Linked List`
