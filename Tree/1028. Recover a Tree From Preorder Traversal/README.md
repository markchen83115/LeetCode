# Leetcode - 1028. Recover a Tree From Preorder Traversal (H-)

[Leetcode](https://leetcode.com/problems/recover-a-tree-from-preorder-traversal/)

We run a preorder depth-first search (DFS) on the `root` of a binary tree.

At each node in this traversal, we output `D` dashes (where `D` is the depth of this node), then we output the value of this node.  If the depth of a node is `D`, the depth of its immediate child is `D + 1`.  The depth of the `root` node is `0`.

If a node has only one child, that child is guaranteed to be **the left child**.

Given the output `traversal` of this traversal, recover the tree and return _its_ `root`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2024/09/10/recover_tree_ex1.png)
> 
> **Input:** traversal = "1-2--3--4-5--6--7"
> **Output:** [1,2,5,3,4,6,7]

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2024/09/10/recover_tree_ex2.png)
> 
> **Input:** traversal = "1-2--3---4-5--6---7"
> **Output:** [1,2,5,3,null,6,null,4,null,7]

**Example 3:**

> ![](https://assets.leetcode.com/uploads/2024/09/10/recover_tree_ex3.png)
> 
> **Input:** traversal = "1-401--349---90--88"
> **Output:** [1,401,null,349,88,90]

**Constraints:**

-   The number of nodes in the original tree is in the range `[1, 1000]`.
-   `1 <= Node.val <= 109`

---
```java
// Java 4m(Beats 67.03%), Time O(N), Space O(N)
class Solution {
    Queue<int[]> queue = new LinkedList<>();    // val, depth
    public TreeNode recoverFromPreorder(String traversal) {
        int n = traversal.length();
        int i = 0;
        while (i < n)
        {
            int depth = 0, val = 0;
            while (i < n && traversal.charAt(i) == '-')
            {
                depth++;
                i++;
            }
            while (i < n && traversal.charAt(i) != '-')
            {
                val = val * 10 + (traversal.charAt(i) - '0');
                i++;
            }
            queue.offer(new int[] {val, depth});
        }

        return dfs(0);
    }

    TreeNode dfs(int depth)
    {
        if (queue.isEmpty())
            return null;
        if (queue.peek()[1] != depth)   // check depth
            return null;
        
        int[] cur = queue.poll();
        TreeNode root = new TreeNode(cur[0]);
        root.left = dfs(depth + 1);
        root.right = dfs(depth + 1);
        
        return root;
    }
}
```
---

參考[【每日一题】1028. Recover a Tree From Preorder Traversal, 6/17/2020 - YouTube](https://youtu.be/IKyFotRqM2w)

我們知道，用樹的先序遍歷可以唯一地重構出這棵樹來。這在`297.Serialize and Deserialize Binary Tree`中曾經提到過。但在那題裡面，先序遍歷的序列化需要包含所有的空節點。在本題所給出的先序遍歷裡，並沒有指明空節點，但是提供了另一個資訊：每個節點的深度。那我們可以利用這個條件來實現相似的重構。

首先我們先預處理字串，使得其變成一個節點數組nodes，每個節點元素包含了value和depth兩個資訊。

我們設計遞歸函數DFS(cur)，表示建構以nodes[cur]為根的子樹。先序遍歷的最大特點是，任何子樹序列的第一個元素cur一定是該子樹的root

通常情況下，第二個元素cur+1就是左子樹的根
但是也有可能root的左節點是空，這樣的話第二元素就不是左子樹的根（因為空節點不會計入序列）。那麼如何判斷呢？
看它們的深度資訊就可以了：

```java
  TreeNode dfs(int depth)
  {
      if (queue.isEmpty())
          return null;
      if (queue.peek()[1] != depth)   // check depth
          return null;

      int[] cur = queue.poll();
      TreeNode root = new TreeNode(cur[0]);
      root.left = dfs(depth + 1);
      root.right = dfs(depth + 1);

      return root;
  }
```


###### tags: `Leetcode` `Tree` `Tree & Sequence`