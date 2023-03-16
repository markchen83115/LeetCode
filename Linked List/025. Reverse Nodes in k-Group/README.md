# Leetcode - 25. Reverse Nodes in k-Group (M+)

[Leetcode](https://leetcode.com/problems/reverse-nodes-in-k-group/description/)

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return _the modified list_.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)
```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)
```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```
**Constraints:**

-   The number of nodes in the list is `n`.
-   `1 <= k <= n <= 5000`
-   `0 <= Node.val <= 1000`

**Follow-up:** Can you solve the problem in `O(1)` extra memory space?

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
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode();
        dummy.next = head;
        int count = 0;
        ListNode begin = dummy;
        while (head != null) {
            count++;
            if (count % k == 0) {
                begin = reverse(begin, head.next);
                head = begin;
            }
            head = head.next;
        }
        return dummy.next;
    }

    public ListNode reverse(ListNode begin, ListNode end) {
        // 0 -> 1 -> 2 -> 3 -> 4 
        // b                   e
        // p    fc

        // 0 <- 1 <- 2 <- 3 -> 4 
        // b    f         p    c    
        ListNode first = begin.next;
        ListNode curr = first;
        ListNode prev = begin;
        while (curr != end) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        begin.next = prev;
        first.next = curr;
        return first;
    }
}
```

```java
// Java Recursive
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        //1. test weather we have more then k node left, if less then k node left we just return head 
        ListNode curr = head;
        int count = 0;
        while (count < k) { 
            if(curr == null) return head;
            curr = curr.next;
            count++;
        }
        // 2.reverse k node at current level 
        ListNode pre = reverseKGroup(curr, k); //pre curr point to the the answer of sub-problem 
        while (count > 0) {  
            ListNode next = head.next; 
            head.next = pre; 
            pre = head; 
            head = next;
            count = count - 1;
        }
        return pre;
    }
}
```

---
> Iteration

計算目前走過的個數`count`, 若`count`為`k`的倍數時, 代表需要翻轉

翻轉時, 需要將頭尾`begin, end`都一同傳進去, 因為只翻轉中間(不含頭尾)
需要維護`prev, first, curr`這三個變數, 才能將翻轉結果與頭尾重新連接
```
翻轉前: 0 -> [1 -> 2 -> 3] -> 4
       bp    fc              e
翻轉後: 0 <- [1 <- 2 <- 3] -> 4
       b     f         p     ce   
連結後: 0 -> [3 -> 2 -> 1] -> 4	
       b     p         f     ce   
```

> Recursive

將前k個翻轉, 並且回傳翻轉後的`head`
將`tail`與下一個遞迴的`head`連結
因此利用遞迴再`k+1 node`開始翻轉k個, 

###### tags: `Leetcode` `Linked List` `Reverse Linked List`
