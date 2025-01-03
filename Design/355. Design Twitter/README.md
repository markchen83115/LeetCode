# Leetcode - 355. Design Twitter (H)

[Leetcode](https://leetcode.com/problems/design-twitter/)

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the `10` most recent tweets in the user's news feed.

Implement the `Twitter` class:

-   `Twitter()` Initializes your twitter object.
-   `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
-   `List<Integer> getNewsFeed(int userId)` Retrieves the `10` most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be **ordered from most recent to least recent**.
-   `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
-   `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.

**Example 1:**

> **Input**
> mod["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"mod]
> mod[mod[mod], mod[1, 5mod], mod[1mod], mod[1, 2mod], mod[2, 6mod], mod[1mod], mod[1, 2mod], mod[1mod]mod]
> **Output**
> mod[null, null, mod[5mod], null, null, mod[6, 5mod], null, mod[5mod]mod]
> 
> **Explanation**
> Twitter twitter = new Twitter();
> twitter.postTweet(1, 5); // User 1 posts a new tweet (id = 5).
> twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> mod[5mod]. return mod[5mod]
> twitter.follow(1, 2);    // User 1 follows user 2.
> twitter.postTweet(2, 6); // User 2 posts a new tweet (id = 6).
> twitter.getNewsFeed(1);  // User 1's news feed should return a list with 2 tweet ids -> mod[6, 5mod]. Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
> twitter.unfollow(1, 2);  // User 1 unfollows user 2.
> twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> mod[5mod], since user 1 is no longer following user 2.

**Constraints:**

-   `1 <= userId, followerId, followeeId <= 500`
-   `0 <= tweetId <= 104`
-   All the tweets have **unique** IDs.
-   At most `3 * 104` calls will be made to `postTweet`, `getNewsFeed`, `follow`, and `unfollow`.
-   A user cannot follow himself.

---
```java
// Java 29ms(Beats 55.24%), Time O(NlogN), Space O(N)
class Twitter {
    HashMap<Integer, HashSet<Integer>> followMap; // userId : List<userId>
    HashMap<Integer, List<int[]>> postMap; // userId : List<{timestamp, tweetId}>
    int timestamp;
    public Twitter() {
        followMap = new HashMap<>();
        postMap = new HashMap<>();
        timestamp = 0;
    }
    
    public void postTweet(int userId, int tweetId) {
        postMap.computeIfAbsent(userId, v -> new ArrayList<>()).add(new int[] {timestamp, tweetId});
        timestamp++;
    }
    
    public List<Integer> getNewsFeed(int userId) {
        List<Integer> list = new ArrayList<>();
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]); // time, tweetId
        if (postMap.containsKey(userId))
            for (int[] p : postMap.get(userId))
                pq.offer(p);
        
        if (followMap.containsKey(userId))
            for (int followed : followMap.get(userId))
                if (postMap.containsKey(followed))
                    for (int[] p : postMap.get(followed))
                        pq.offer(p);
        
        while (!pq.isEmpty() && list.size() < 10)
            list.add(pq.poll()[1]);
        
        return list;
    }
    
    public void follow(int followerId, int followeeId) {
        followMap.computeIfAbsent(followerId, v -> new HashSet<>()).add(followeeId);
    }
    
    public void unfollow(int followerId, int followeeId) {
        if (followMap.containsKey(followerId))
            followMap.get(followerId).remove(followeeId);
    }
}
```
---

用`HashMap<Integer, HashSet<Integer>>` 儲存每個人的follow清單
`(userId : List<userId>)`

用`HashMap<Integer, List<intmod[mod]>>` 儲存每個人的posts, 
並且用全域變數`timestamp`紀錄時間 `(userId : List<{timestamp, tweetId}>)`

當要輸出10筆posts時, 將有follow的人(包含自己)的所有貼文放入優先隊列
以時間大排到小, 輸出前10筆posts


###### tags: `Leetcode` `Design`