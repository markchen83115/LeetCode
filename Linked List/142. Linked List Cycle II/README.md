# Leetcode - 142. Linked List Cycle II (M+)

[Leetcode](https://leetcode.com/problems/linked-list-cycle-ii/)

Given the `head` of a linked list, return _the node where the cycle begins. If there is no cycle, return _`null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)
> 
> **Input:** head = [3,2,0,-4], pos = 1
> **Output:** tail connects to node index 1
> **Explanation:** There is a cycle in the linked list, where tail connects to the second node.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)
> 
> **Input:** head = [1,2], pos = 0
> **Output:** tail connects to node index 0
> **Explanation:** There is a cycle in the linked list, where tail connects to the first node.

**Example 3:**

> ![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)
> 
> **Input:** head = [1], pos = -1
> **Output:** no cycle
> **Explanation:** There is no cycle in the linked list.

**Constraints:**

-   The number of the nodes in the list is in the range `[0, 104]`.
-   `-105 <= Node.val <= 105`
-   `pos` is `-1` or a **valid index** in the linked-list.

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?

---
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head, slow = head;
        if (head == null || head.next == null)
            return null;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) break;
        }

        if (fast != slow)
            return null;
        
		// 此時為相遇點, 
        // 一個從相遇點出發, 一個從起點出發
        // 再次相遇點 = 環的入口
        slow = head;
        while (slow != fast)
        {
            slow = slow.next;
            fast = fast.next;
        }

        return slow;
    }
}
```
---
[142. Linked List Cycle II, 10/16/2020 - YouTube](https://youtu.be/kZP8Cij1fxk)

從第141題已知，用快慢指針的方法可以判斷linked list是否存在環。

假設兩個指針相遇時，快指針走過了m步進入環的入口然後轉了k1圈（每圈n步）又多p步；同理，慢指針走過了m步進入環的入口然後轉了k2圈又多p步。由於兩者是兩倍關係，所以 m+k1*n+p = 2*(m+k2*n+p)。

化簡之後得到 m = (k1-2k2)*n-p，變換 p+m = (k1-2k2)*n

因為慢指針目前已經比整數圈多走了p步，結合這個數學式子，這說明如果慢指針再走m步的話，又會湊成整數圈，即到了環的入口。怎麼確定m呢？只要另開一個指針從head開始與慢指針一齊走，它們相遇的地方就是環的入口。

本題的演算法還有一個非常巧妙的應用：287. Find the Duplicate Number


###### tags: `Leetcode` `Linked List` `Two Pointers`