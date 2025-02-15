# Leetcode - 082. Remove Duplicates from Sorted List II (M+)

[Leetcode](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

Given the `head` of a sorted linked list, _delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list_. Return _the linked list **sorted** as well_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)
> 
> **Input:** head = [1,2,3,3,4,4,5]
> **Output:** [1,2,5]

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)
> 
> **Input:** head = [1,1,1,2,3]
> **Output:** [2,3]

**Constraints:**

-   The number of nodes in the list is in the range `[0, 300]`.
-   `-100 <= Node.val <= 100`
-   The list is guaranteed to be **sorted** in ascending order.

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
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null)
            return null;
        ListNode dummy = new ListNode();
        dummy.next = head;
        ListNode fast = head.next, slow = head, pre = dummy;
        while (fast != null && fast.val == slow.val)
        {
            fast = fast.next;
            slow = slow.next;
        }
        
        if (pre.next != slow)  // duplicate
        {
            pre.next = deleteDuplicates(fast);
        }
        else
        {
            slow.next = deleteDuplicates(fast);
        }

        return dummy.next;
    }
}
```
---

```
//   1 2 3
// p s f
// p   s f


//   1 1 1 2 3
// p     s f
// p       s f
```
透過`pre.next == slow`查看是否有重複
若有重複, 則將`pre.next = deleteDuplicates(fast)`來去調重複
若沒有重複,則將`pre.next = slow`保持原樣, 因此繼續檢查`slow.next = deleteDuplicates(fast)`


###### tags: `Leetcode` `Linked List`