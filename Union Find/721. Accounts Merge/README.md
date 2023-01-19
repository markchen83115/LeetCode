# Leetcode - 721. Accounts Merge (M+)

[Leetcode](https://leetcode.com/problems/accounts-merge/description/)

Given a list of `accounts` where each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are **emails** representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in **any order**.

**Example 1:**
```
Input: accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
Output: [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
Explanation:
The first and second John's are the same person as they have the common email "johnsmith@mail.com".
The third John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.
```
**Example 2:**
```
Input: accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]]
Output: [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]]
```
**Constraints:**

-   `1 <= accounts.length <= 1000`
-   `2 <= accounts[i].length <= 10`
-   `1 <= accounts[i][j].length <= 30`
-   `accounts[i][0]` consists of English letters.
-   `accounts[i][j] (for j > 0)` is a valid email.

---

```java
// Java 43ms
class Solution {
    HashMap<String, String> mailRootMap = new HashMap<>();  // email -> root mail
    HashMap<String, String> mailNameMap = new HashMap<>();  // email -> name
    HashMap<String, TreeSet<String>> retMap = new HashMap<>();  // email -> mail list
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        List<List<String>> retList = new ArrayList<>();
        for (List<String> account : accounts) {
            for (int i = 1; i < account.size(); i++) {
                mailRootMap.put(account.get(i), account.get(i));
                mailNameMap.put(account.get(i), account.get(0));
            }
        }
        for (List<String> account : accounts) {
            String root = findRoot(account.get(1));
            for (int i = 2; i < account.size(); i++) {
                mailRootMap.put(findRoot(account.get(i)), root);
            }
        }

        for (String mail : mailRootMap.keySet()) {
            String root = findRoot(mail);
            if (!retMap.containsKey(root))
                retMap.put(root, new TreeSet<String>());
            retMap.get(root).add(mail);
        }
        for (String root : retMap.keySet()) {
            List<String> list = new ArrayList<>();
            list.add(mailNameMap.get(root));
            for (String mail : retMap.get(root)) {
                list.add(mail);
            }
            retList.add(list);
        }
        return retList;
    }

    public String findRoot(String x) {
        if (mailRootMap.get(x) != x) {
            mailRootMap.put(x, findRoot(mailRootMap.get(x)));
        }
        return mailRootMap.get(x);
    }
}
```

```java
// Java 40ms
class Solution {
    HashMap<String, String> mailRootMap = new HashMap<>();  // email -> root mail
    HashMap<String, String> mailNameMap = new HashMap<>();  // email -> name
    HashMap<String, TreeSet<String>> retMap = new HashMap<>();  // email -> mail list
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        List<List<String>> retList = new ArrayList<>();
        for (List<String> account : accounts) {
            String name = account.get(0);
            String firstMail = account.get(1);
            if (!mailRootMap.containsKey(firstMail)) {
                mailRootMap.put(firstMail, firstMail);
                mailNameMap.put(firstMail, name);
            }
            
            for (int i = 2; i < account.size(); i++) {
                String mail = account.get(i);
                if (!mailRootMap.containsKey(mail)) {
                    mailRootMap.put(mail, mail);
                    mailNameMap.put(mail, name);
                }
                if (findRoot(mail) != findRoot(firstMail)) {
                    union(mail, firstMail);
                }
            }
        }

        for (String mail : mailRootMap.keySet()) {
            String root = findRoot(mail);
            if (!retMap.containsKey(root))
                retMap.put(root, new TreeSet<String>());
            retMap.get(root).add(mail);
        }
        for (String root : retMap.keySet()) {
            List<String> list = new ArrayList<>();
            list.add(mailNameMap.get(root));
            list.addAll(retMap.get(root));
            retList.add(list);
        }
        return retList;
    }

    public String findRoot(String x) {
        if (mailRootMap.get(x) != x) {
            mailRootMap.put(x, findRoot(mailRootMap.get(x)));
        }
        return mailRootMap.get(x);
    }

    public void union(String x, String y) {
        x = mailRootMap.get(x);
        y = mailRootMap.get(y);
        if (x.compareTo(y) < 0) {
            mailRootMap.put(y, x);
        } else {
            mailRootMap.put(x, y);
        }
    }
}
```


```java
// Java 24ms
class Solution {
    class UnionFind {
        int[] parent;
        int[] weight;
        
        public UnionFind(int num) {
            parent = new int[num];
            weight = new int[num];
            
            for(int i =  0; i < num; i++) {
                parent[i] = i;
                weight[i] = 1;
            }
        }
        
        public void union(int a, int  b) {
            int rootA = root(a);
            int rootB = root(b);
            
            if (rootA == rootB) {
                return;
            }
            
            if (weight[rootA] > weight[rootB]) {
                parent[rootB] = rootA;
                weight[rootA] += weight[rootB];
            } else {
                parent[rootA] = rootB;
                weight[rootB] += weight[rootA];
            }
        }
        
        public int root(int a) {
            if (parent[a] == a) {
                return a;
            }
            
            parent[a] = root(parent[a]);
            return parent[a];
        }
    }

    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        int size = accounts.size();

        UnionFind uf = new UnionFind(size);

        // prepare a hash with unique email address as key and index in accouts as value
        HashMap<String, Integer> emailToId = new  HashMap<>();
        for(int i = 0; i < size; i++) {
            List<String> details = accounts.get(i);
            for(int j = 1; j < details.size(); j++) {
                String email = details.get(j);
                
				// if we have already seen this email before, merge the account  "i" with previous account
				// else add it to hash
                if (emailToId.containsKey(email)) {
                    uf.union(i, emailToId.get(email));
                } else  {
                    emailToId.put(email, i);
                }
            }
        }
        
        // prepare a hash with index in accounts as key and list of unique email address for that account as value
        HashMap<Integer, List<String>> idToEmails = new HashMap<>();
        for(String key : emailToId.keySet()) {
            int root = uf.root(emailToId.get(key));
            
            if (!idToEmails.containsKey(root)) {
                idToEmails.put(root, new ArrayList<String>());
            }
            
            idToEmails.get(root).add(key);
        }
        
        // collect the emails from idToEmails, sort it and add account name at index 0 to get the final list to add to final return List
        List<List<String>> mergedDetails =  new ArrayList<>();
        for(Integer id : idToEmails.keySet()) {
            List<String> emails =  idToEmails.get(id);
            Collections.sort(emails);
            emails.add(0, accounts.get(id).get(0));
            
            mergedDetails.add(emails);
        }
        
        return  mergedDetails;
    }
}
```



---

[wisdompeak/YouTube](https://www.youtube.com/watch?v=SaDPCgT-EbQ)


```javascript
a b c // now b, c have parent a
d e f // now e, f have parent d
g a d // now abc, def all merged to group g

parents populated after parsing 1st account: a b c
a->a
b->a
c->a

parents populated after parsing 2nd account: d e f
d->d
e->d
f->d

parents populated after parsing 3rd account: g a d
g->g
a->g
d->g
```


###### tags: `Leetcode` `Union Find`