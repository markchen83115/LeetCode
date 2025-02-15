# Leetcode - 092. Reverse Linked List II (M+)

[Leetcode](https://leetcode.com/problems/reverse-linked-list-ii/)

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return _the reversed list_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)
> 
> **Input:** head = [1,2,3,4,5], left = 2, right = 4
> **Output:** [1,4,3,2,5]

**Example 2:**

> **Input:** head = [5], left = 1, right = 1
> **Output:** [5]

**Constraints:**

-   The number of nodes in the list is `n`.
-   `1 <= n <= 500`
-   `-500 <= Node.val <= 500`
-   `1 <= left <= right <= n`

**Follow up:** Could you do it in one pass?

---
```java
// Java 0ms(Beats 100.00%), Time O(N), Space O(N)
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (left == right)
            return head;
        ListNode dummy = new ListNode(), tail;
        dummy.next = head;
        ListNode pre = dummy;
        ListNode l = head, r = head;
        for (int i = left; i < right; i++)
            r = r.next;
        for (int i = 1; i < left; i++)
        {
            pre = l;
            l = l.next;
            r = r.next;
        }
        tail = r.next;

        for (int i = left; i <= right; i++)
        {
            ListNode next = l.next;
            l.next = tail;
            tail = l;
            l = next;
        }
        pre.next = r;
        
        return dummy.next;
    }
}
// 1 2 3 4 5
// p l   r t
```
---

本題的基本想法很明顯，我們需要將原始列表拆分為三個部分。然後對中間一部分進行鍊錶反轉操作（參考 206.Reverse-Linked-List），最後將三個部分拼接起來。

要注意的是left和right可能很極限，也就是說原題可能本質上是反轉整個鍊錶。所以方便的處理方法是前面新增一個dummy節點，最後也虛擬地認為有NULL節點。這樣我們就一定能夠把整個鍊錶分為實實在在的三個部分。然後應用上述的演算法。


###### tags: `Leetcode` `Linked List` `Reverse Linked List`