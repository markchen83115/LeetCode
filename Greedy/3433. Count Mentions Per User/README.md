# Leetcode - 3433. Count Mentions Per User (M)

[Leetcode](https://leetcode.com/problems/count-mentions-per-user/)

You are given an integer `numberOfUsers` representing the total number of users and an array `events` of size `n x 3`.

Each `events[i]` can be either of the following two types:

1.  **Message Event:** `["MESSAGE", "timestamp i", "mentions_string i"]`
    -   This event indicates that a set of users was mentioned in a message at `timestamp i`.
    -   The `mentions_string i` string can contain one of the following tokens:
        -   `id<number>`: where `<number>` is an integer in range `[0,numberOfUsers - 1]`. There can be **multiple** ids separated by a single whitespace and may contain duplicates. This can mention even the offline users.
        -   `ALL`: mentions **all** users.
        -   `HERE`: mentions all **online** users.
2.  **Offline Event:** `["OFFLINE", "timestamp i", "id i"]`
    -   This event indicates that the user `id i` had become offline at `timestamp i` for **60 time units**. The user will automatically be online again at time `timestamp i + 60`.

Return an array `mentions` where `mentions[i]` represents the number of mentions the user with id `i` has across all `MESSAGE` events.

All users are initially online, and if a user goes offline or comes back online, their status change is processed _before_ handling any message event that occurs at the same timestamp.

**Note** that a user can be mentioned **multiple** times in a **single** message event, and each mention should be counted **separately**.

**Example 1:**

> **Input:** numberOfUsers = 2, events = [["MESSAGE","10","id1 id0"],["OFFLINE","11","0"],["MESSAGE","71","HERE"]]
> 
> **Output:** [2,2]
> 
> **Explanation:**
> 
> Initially, all users are online.
> 
> At timestamp 10, `id1` and `id0` are mentioned. `mentions = [1,1]`
> 
> At timestamp 11, `id0` goes **offline.**
> 
> At timestamp 71, `id0` comes back **online** and `"HERE"` is mentioned. `mentions = [2,2]`

**Example 2:**

> **Input:** numberOfUsers = 2, events = [["MESSAGE","10","id1 id0"],["OFFLINE","11","0"],["MESSAGE","12","ALL"]]
> 
> **Output:** [2,2]
> 
> **Explanation:**
> 
> Initially, all users are online.
> 
> At timestamp 10, `id1` and `id0` are mentioned. `mentions = [1,1]`
> 
> At timestamp 11, `id0` goes **offline.**
> 
> At timestamp 12, `"ALL"` is mentioned. This includes offline users, so both `id0` and `id1` are mentioned. `mentions = [2,2]`

**Example 3:**

> **Input:** numberOfUsers = 2, events = [["OFFLINE","10","0"],["MESSAGE","12","HERE"]]
> 
> **Output:** [0,1]
> 
> **Explanation:**
> 
> Initially, all users are online.
> 
> At timestamp 10, `id0` goes **offline.**
> 
> At timestamp 12, `"HERE"` is mentioned. Because `id0` is still offline, they will not be mentioned. `mentions = [0,1]`

**Constraints:**

-   `1 <= numberOfUsers <= 100`
-   `1 <= events.length <= 100`
-   `events[i].length == 3`
-   `events[i][0]` will be one of `MESSAGE` or `OFFLINE`.
-   `1 <= int(events[i][1]) <= 105`
-   The number of `id<number>` mentions in any `"MESSAGE"` event is between `1` and `100`.
-   `0 <= <number> <= numberOfUsers - 1`
-   It is **guaranteed** that the user id referenced in the `OFFLINE` event is **online** at the time the event occurs.

---
```java
// Java 35ms(Beats 27.17%), Time O(NlogN), Space O(N)
class Solution {
    public int[] countMentions(int n, List<List<String>> events) {
        int[] ret = new int[n];
        Collections.sort(events, (a, b) -> 
            Integer.parseInt(a.get(1)) == Integer.parseInt(b.get(1)) ? b.get(0).compareTo(a.get(0)) : Integer.parseInt(a.get(1)) - Integer.parseInt(b.get(1)));

        HashSet<Integer> here = new HashSet<>();
        Queue<int[]> offlineQueue = new LinkedList<>();
        for (int i = 0; i < n; i++)
            here.add(i);

        for (List<String> e : events)
        {
            String type = e.get(0); // MESSAGE OFFLINE
            int time = Integer.valueOf(e.get(1));
            String mention = e.get(2);
            // Add online
            while (!offlineQueue.isEmpty() && offlineQueue.peek()[0] <= time)
                here.add(offlineQueue.poll()[1]);
            
            if (type.equals("OFFLINE"))
            {
                int id = Integer.valueOf(mention);
                offlineQueue.offer(new int[] {time + 60, id});
                here.remove(id);
                continue;
            }

            // MESSAGE
            switch(mention)
            {
                case "ALL":
                    for (int i = 0; i < n; i++)
                        ret[i]++;
                    break;
                case "HERE":
                    for (int i : here)
                        ret[i]++;
                    break;
                default:
                    String[] users = mention.split(" ");
                    for (String u : users)
                    {
                        int id = Integer.valueOf(u.substring(2));
                        ret[id]++;
                    }
            }
        }

        return ret;
    }
}
```
---

注意`Integer.valufOf()`與`Integer.parseInt()`有差別
```
Integer.valueOf()使用了Integer緩存, 可以緩存-128到127之間的整數
Integer.parseInt()不使用緩存, 每次都會傳回一個新的int類型值
```
因此此題必須用`Integer.parseInt()`, 否則比較大小會有錯誤


將`events`依`timestamp`排序, 並注意若`timestamp`相同`OFFLINE`要在`MESSAGE`前面
`offline`的人放入`queue`中, 並時時檢查是否已到`online`時刻



###### tags: `Leetcode` `Greedy`