# Leetcode - 2. Add Two Numbers (M)

[Leetcode](https://leetcode.com/problems/add-two-numbers/description/)

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```
**Example 2:**
```
Input: l1 = [0], l2 = [0]
Output: [0]
```
**Example 3:**
```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```
**Constraints:**

-   The number of nodes in each linked list is in the range `[1, 100]`.
-   `0 <= Node.val <= 9`
-   It is guaranteed that the list represents a number that does not have leading zeros.

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = new ListNode();
        ListNode dummy = head;
        int add = 0;
        int sum = 0;
        while (l1 != null && l2 != null) {
            sum  = l1.val + l2.val + add;
            head.next = new ListNode(sum % 10);
            add = sum / 10;
            head = head.next;
            l1 = l1.next;
            l2 = l2.next;
        }

        while (l1 != null) {
            sum  = l1.val + add;
            head.next = new ListNode(sum % 10);
            add = sum / 10;
            head = head.next;
            l1 = l1.next;
        }

        while (l2 != null) {
            sum  = l2.val + add;
            head.next = new ListNode(sum % 10);
            add = sum / 10;
            head = head.next;
            l2 = l2.next;
        }

        if (add > 0) {
            head.next = new ListNode(1);
        }

        return dummy.next;
    }
}
```



---

###### tags: `Leetcode` `Linked List`
