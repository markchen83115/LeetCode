# Leetcode - 206. Reverse Linked List (M)

[Leetcode](https://leetcode.com/problems/reverse-linked-list/description/)

Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)
```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)
```
Input: head = [1,2]
Output: [2,1]
```
**Example 3:**
```
Input: head = []
Output: []
```
**Constraints:**

-   The number of nodes in the list is the range `[0, 5000]`.
-   `-5000 <= Node.val <= 5000`

**Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?

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
// Java Iteration
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode newHead = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = newHead;
            newHead = head;
            head = next;
        }
        return newHead;
    }
}
```

```java
// Java Recursive
class Solution {
    public ListNode reverseList(ListNode head) {
        return reverseList(head, null);
    }

    public ListNode reverseList(ListNode head, ListNode newHead) {
        if (head == null) {
            return newHead;
        }
        ListNode next = head.next;
        head.next = newHead;
        return reverseList(next, head);
    }
}
```

---

###### tags: `Leetcode` `Linked List` `Reverse Linked List`
