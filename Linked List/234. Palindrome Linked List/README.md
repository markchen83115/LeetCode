# Leetcode - 234. Palindrome Linked List (M)

[Leetcode](https://leetcode.com/problems/palindrome-linked-list/description/)

Given the `head` of a singly linked list, return `true`_ if it is a _ 

_palindrome_

_ or _`false`_ otherwise_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)
```
Input: head = [1,2,2,1]
Output: true
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)
```
Input: head = [1,2]
Output: false
```
**Constraints:**

-   The number of nodes in the list is in the range `[1, 105]`.
-   `0 <= Node.val <= 9`

**Follow up:** Could you do it in `O(n)` time and `O(1)` space?

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
// Java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode fast = head, slow = head;
        // spilt at mid
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }

        // 1 -> 1 -> 2 -> 1 -> null 
        //           s          f
        if (fast != null)   //odd, make slow smaller
            slow = slow.next; 

        // reverse from mid to end
        // 1 -> 1    null <- 2 <- 1           
        // h                      s
        ListNode newHead = null;
        while (slow != null) {
            ListNode next = slow.next;
            slow.next = newHead;
            newHead = slow;
            slow = next;
        }
        slow = newHead;

        // compare
        while (slow != null) {
            if (head.val != slow.val)
                return false;
            head = head.next;
            slow = slow.next;
        }

        return true;
    }
}
```
---

[JS, Python, Java, C++ | Easy Floyd's + Reversal Solution w/ Explanation](https://leetcode.com/problems/palindrome-linked-list/solutions/1137027/js-python-java-c-easy-floyd-s-reversal-solution-w-explanation/)

> Step 1:
![](https://i.imgur.com/tvJRkwn.png)
>Step 2:
![](https://i.imgur.com/mDddOLc.png)
>Step 3:
![](https://i.imgur.com/XAa5RLA.png)
>Even Length:
![](https://i.imgur.com/8NinDle.png)



###### tags: `Leetcode` `Linked List`
