# Leetcode - 61. Rotate List (M)

[Leetcode](https://leetcode.com/problems/rotate-list/description/)

Given the `head` of a linked list, rotate the list to the right by `k` places.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg)
```
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg)
```
Input: head = [0,1,2], k = 4
Output: [2,0,1]
```
**Constraints:**

-   The number of nodes in the list is in the range `[0, 500]`.
-   `-100 <= Node.val <= 100`
-   `0 <= k <= 2 * 109`

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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) return head;
        ListNode dummy = new ListNode();
        dummy.next = head;
        ListNode fast = dummy, slow = dummy;
        // count total
        int n = 0;
        while (fast.next != null) {
            fast = fast.next;
            n++;
        }
        // get the [n - k % n] node
        for (int j = k % n; j < n; j++) {
            slow = slow.next;
        }
        // link tail to head
        fast.next = dummy.next;
        dummy.next = slow.next;
        slow.next = null;

        return dummy.next;

    }
}
```

---

linked list長度為`n`, 則rotate `n`次會變回原本的linked list
因此先算出長度`n`, 預先處理rotate`k % n`次
再算出從哪裡開始切斷, 並接到`head`


###### tags: `Leetcode` `Linked List`
