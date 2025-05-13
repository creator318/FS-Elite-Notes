#Coding/Trees/Traversal  #Coding/DFS #Coding/Backtracking 

Imagine a company where each employee has a performance score, and the organizational chart is structured as a binary tree with the CEO at the top. 
An employee is considered "outstanding" if, along the chain of command from the CEO down to that employee, no one has a performance score higher than that employee's score. Your task is to determine the total number of outstanding employees in the company.

Sample Input-1:
------
```
3 1 4 3 -1 1 5
```

Sample Output-1:
------
```
4
```

Explanation:
------
```
    3
   / \
  1   4
 /   / \
3   1   5
```
- The CEO (score 3) is automatically outstanding.
- The employee with score 4, whose chain is \[3,4], is outstanding because 4 is higher than 3.
- The employee with score 5, following the path \[3,4,5], is outstanding as 5 is the highest so far.
- The subordinate with score 3, along the path \[3,1,3], is outstanding because none of the managers in that chain have a score exceeding 3.


Sample Input-2: 
------
```
3 3 -1 4 2
```

Sample Output-2: 
------
```
3
```

Explanation:
------
```
    3
   / 
  3
 / \
4   2
```
- The CEO (score 3) is outstanding.
- The subordinate with score 3 (chain: \[3,3]) is outstanding.
- The employee with score 2 (chain: \[3,3,2]) is not outstanding because the managers have higher scores.

## Solution:

```java
import java.util.*;

class Node {
  int val;
  Node l=null, r=null;
  
  Node(int val) {
    this.val = val;
  }
}

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int[] vals = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
  
    System.out.println(outstandingCount(vals));

    sc.close();
  }
    
  private static int outstandingCount(int[] vals) {
    Node root = buildTree(vals);

    return dfs(root, root.val, 0);
  }

  private static Node buildTree(int[] vals) {
    if (vals.length == 0) return null;
    
    Node root = new Node(vals[0]);
    Queue<Node> q = new LinkedList<>();
    q.add(root);
    
    int i = 0;
    while (i<vals.length-1) {
      Node curr = q.remove();
      
      if (vals[++i] != -1) {
        curr.l = new Node(vals[i]);
        q.add(curr.l);
      }
      
      if (i<vals.length-1 && vals[++i] != -1) {
        curr.r = new Node(vals[i]);
        q.add(curr.r);
      }
    }
    
    return root;
  }

  private static void dfs(Node root, int max, int count) {
    if (root==null) return count;
    
    int prev = max;
    max = Math.max(max, root.val);
    
    if (root.val>=prev) count++;
    
    count = dfs(root.l, max, count);
    count = dfs(root.r, max, count);
    
    max = prev;
    
    return count;
  }
}
```