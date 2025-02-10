# Leetcode - 3447. Assign Elements to Groups with Constraints (M+)

[Leetcode](https://leetcode.com/problems/assign-elements-to-groups-with-constraints/)

You are given an integer array `groups`, where `groups[i]` represents the size of the `ith` group. You are also given an integer array `elements`.

Your task is to assign **one** element to each group based on the following rules:

-   An element `j` can be assigned to a group `i` if `groups[i]` is **divisible** by `elements[j]`.
-   If there are multiple elements that can be assigned, assign the element with the **smallest index** `j`.
-   If no element satisfies the condition for a group, assign -1 to that group.

Return an integer array `assigned`, where `assigned[i]` is the index of the element chosen for group `i`, or -1 if no suitable element exists.

**Note**: An element may be assigned to more than one group.

**Example 1:**

> **Input:** groups = [8,4,3,2,4], elements = [4,2]
> 
> **Output:** [0,0,-1,1,0]
> 
> **Explanation:**
> 
> -   `elements[0] = 4` is assigned to groups 0, 1, and 4.
> -   `elements[1] = 2` is assigned to group 3.
> -   Group 2 cannot be assigned any element.

**Example 2:**

> **Input:** groups = [2,3,5,7], elements = [5,3,3]
> 
> **Output:** [-1,1,0,-1]
> 
> **Explanation:**
> 
> -   `elements[1] = 3` is assigned to group 1.
> -   `elements[0] = 5` is assigned to group 2.
> -   Groups 0 and 3 cannot be assigned any element.

**Example 3:**

> **Input:** groups = [10,21,30,41], elements = [2,1]
> 
> **Output:** [0,1,0,1]
> 
> **Explanation:**
> 
> `elements[0] = 2` is assigned to the groups with even values, and `elements[1] = 1` is assigned to the groups with odd values.

**Constraints:**

-   `1 <= groups.length <= 105`
-   `1 <= elements.length <= 105`
-   `1 <= groups[i] <= 105`
-   `1 <= elements[i] <= 105`

---
```java
// Java 92ms(Beats 100.00%), Time O(N), Space O(N)
class Solution {
    public int[] assignElements(int[] groups, int[] elements) {
        int[] index = new int[100005];
        Arrays.fill(index, -1);
        for (int i = 0; i < elements.length; i++)
            if (index[elements[i]] == -1)
            {
                for (int e = elements[i]; e <= 100000; e += elements[i])
                    if (index[e] == -1)
                        index[e] = i;
            }

        int n = groups.length;
        for (int i = 0; i < n; i++)
            groups[i] = index[groups[i]];

        return groups;
    }
}
```
```java
// Java 優化 6ms(Beats 100.00%), Time O(NlogN), Space O(N)
class Solution {
    public int[] assignElements(int[] groups, int[] elements) {
        int n = 0;
        for (int g : groups)
            n = Math.max(n, g);
            
        int[] index = new int[n + 1];  // 優化
        Arrays.fill(index, -1);
        for (int i = 0; i < elements.length; i++)
        {
            // 優化
            if (elements[i] > n)
                continue;
            if (index[elements[i]] != -1)
                continue;
            
            for (int e = elements[i]; e <= n; e += elements[i])
                if (index[e] == -1)
                    index[e] = i;
        }

        for (int i = 0; i < groups.length; i++)
            groups[i] = index[groups[i]];

        return groups;
    }
}
```
---

突破點在於groups裡的元素的數值不超過1e5.在這個範圍是，如果枚舉所有1的倍數，然後枚舉所有2的倍數，然後枚舉所有3的倍數，直至枚舉n的倍數，那麼總共的時間複雜度是`n+n/2+n/3+...n/n = n*(1+1/2+n/3+...n/n = n*(1+1/2+n/3+...n/n = n*(1+1/2+n/3(n-73)`所以本題可以用暴力枚舉

所以本題的演算法很簡單。我們開一個長度為1e5的陣列assign，來記錄每個自然數最早能被哪個element所assign。我們依序考察element裡的每個元素，比如說`elements[j]=x`，然後枚舉x的所有倍數（直至1e5），比如說kx，那樣就有`assign[kx] = j`，當然根據題意，我們對於每個assign我們只更新一次

最後根據groups的數值，從assgin裡把答案拷貝過去即可



###### tags: `Leetcode` `Others` `Enumeration`