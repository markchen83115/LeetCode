# Leetcode - 2130. Maximum Twin Sum of a Linked List (M-)

[Leetcode](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/)

In a linked list of size `n`, where `n` is **even**, the `ith` node (**0-indexed**) of the linked list is known as the **twin** of the `(n-1-i)th` node, if `0 <= i <= (n / 2) - 1`.

-   For example, if `n = 4`, then node `0` is the twin of node `3`, and node `1` is the twin of node `2`. These are the only nodes with twins for `n = 4`.

The **twin sum **is defined as the sum of a node and its twin.

Given the `head` of a linked list with even length, return _the **maximum twin sum** of the linked list_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/12/03/eg1drawio.png)
> 
> **Input:** head = [5,4,2,1]
> **Output:** 6
> **Explanation:**
> Nodes 0 and 1 are the twins of nodes 3 and 2, respectively. All have twin sum = 6.
> There are no other nodes with twins in the linked list.
> Thus, the maximum twin sum of the linked list is 6. 

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/12/03/eg2drawio.png)
> 
> **Input:** head = [4,2,2,3]
> **Output:** 7
> **Explanation:**
> The nodes with twins present in this linked list are:
> - Node 0 is the twin of node 3 having a twin sum of 4 + 3 = 7.
> - Node 1 is the twin of node 2 having a twin sum of 2 + 2 = 4.
> Thus, the maximum twin sum of the linked list is max(7, 4) = 7. 

**Example 3:**

> ![](https://assets.leetcode.com/uploads/2021/12/03/eg3drawio.png)
> 
> **Input:** head = [1,100000]
> **Output:** 100001
> **Explanation:**
> There is only one node with a twin in the linked list having twin sum of 1 + 100000 = 100001.

**Constraints:**

-   The number of nodes in the list is an **even** integer in the range `[2, 105]`.
-   `1 <= Node.val <= 105`

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
    public int pairSum(ListNode head) {
        int ret = 0;
        ListNode fast = head, slow = head, pre = null, newHead = null;
        while (fast != null && fast.next != null)
        {
            pre = slow;
            slow = slow.next;
            fast = fast.next.next;
            pre.next = newHead;
            newHead = pre;
        }

        while (slow != null)
        {
            ret = Math.max(ret, slow.val + newHead.val);
            slow = slow.next;
            newHead = newHead.next;
        }

        return ret;
    }
}
```
---

```
// 1 2 3 4
// p s f

// 1 2 3 4
//   p s   f
```
用fast, slow持續往後走, 直到fast走到底
在這過程中將第一個到slow的前一個都翻轉過來
最後再兩兩相加的最大值即為答案


###### tags: `Leetcode` `Linked List`