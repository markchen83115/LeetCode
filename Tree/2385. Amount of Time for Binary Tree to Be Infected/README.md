# Leetcode - 2385. Amount of Time for Binary Tree to Be Infected (M)

[Leetcode](https://leetcode.com/problems/amount-of-time-for-binary-tree-to-be-infected/description/)

You are given the `root` of a binary tree with **unique** values, and an integer `start`. At minute `0`, an **infection** starts from the node with value `start`.

Each minute, a node becomes infected if:

-   The node is currently uninfected.
-   The node is adjacent to an infected node.

Return _the number of minutes needed for the entire tree to be infected._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/06/25/image-20220625231744-1.png)
```
Input:root = [1,5,3,null,4,10,6,9,2], start = 3
Output:4
Explanation:The following nodes are infected during:
- Minute 0: Node 3
- Minute 1: Nodes 1, 10 and 6
- Minute 2: Node 5
- Minute 3: Node 4
- Minute 4: Nodes 9 and 2
It takes 4 minutes for the whole tree to be infected so we return 4.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2022/06/25/image-20220625231812-2.png)
```
Input: root = [1], start = 1
Output: 0
Explanation: At minute 0, the only node in the tree is infected so we return 0.
```
**Constraints:**

-   The number of nodes in the tree is in the range `[1, 105]`.
-   `1 <= Node.val <= 105`
-   Each node has a **unique** value.
-   A node with a value of `start` exists in the tree.

---
```java
// Java BFS 136ms(Beats 31.94%), Time O(N), Space O(N)
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    
    public int amountOfTime(TreeNode root, int start) {
        HashMap<Integer, List<Integer>> next = new HashMap<>();
        // transfer Tree to Graph
        toGraph(root, null, next);

        // BFS
        Queue<Integer> queue = new LinkedList<>();
        HashSet<Integer> visited = new HashSet<>();
        int ret = -1;
        queue.offer(start);
        visited.add(start);

        while (!queue.isEmpty())
        {
            int size = queue.size();
            ret++;
            for (int i = 0; i < size; i++)
            {
                int curr = queue.poll();
                visited.add(curr);
                for (int nxt : next.get(curr))
                    if (!visited.contains(nxt))
                    {
                        queue.offer(nxt);
                    }
            }
        }
        return ret;
    }

    void toGraph(TreeNode root, TreeNode parent, HashMap<Integer, List<Integer>> next)
    {
        if (root == null)
            return;
        if (!next.containsKey(root.val))
            next.put(root.val, new ArrayList<Integer>());

        if (parent != null)
            next.get(root.val).add(parent.val); 

        if (root.left != null)
        {
            next.get(root.val).add(root.left.val); 
            toGraph(root.left, root, next);
        }
        if (root.right != null)
        {
            next.get(root.val).add(root.right.val); 
            toGraph(root.right, root, next);
        }
    }
}
```
---

1. 先將Tree轉成Graph, 紀錄每個點的相鄰節點
2. 使用BFS一層一層向外擴散, 計算擴散完畢總共需要幾回合

###### tags: `Leetcode` `Tree` `BFS`