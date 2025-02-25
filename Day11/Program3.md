#Coding/Trees/Traversal 

Imagine you are designing a network of secret corridors in an ancient castle. 
Each chamber in the castle leads to at most two other chambers through hidden passageways, forming a branching layout. 
The castle’s "longest secret route" is defined as the maximum number of corridors  you must traverse to get from one chamber to another (without repeating the corridor). This route may or may not pass through the main entry chamber.

Your task is to compute the length of longest secret route between two chambers which is represented by the number of corridors between them.

Sample Input-1:
------
```
1 2 3 4 5 
```

Sample Output-1:
------
```
3
```

Explanation:
------
```
    1
   / \
  2   3
 / \
4   5
```
The longest secret route from chamber 4 to chamber 3 (alternatively, from chamber 5 to chamber 3) along the route: 4 → 2 → 1 → 3
This path has 3 corridors (4–2, 2–1, 1–3), so the length is 3.

Sample Input-2:
------
```
1 -1 2 3 4
```

Sample Output-2:
------
```
2
```

Explanation:
------
```
 1
  \
   2
  / \
 3   4
```
The longest secret route from chamber 3 to chamber 4 (alternatively, from chamber 1 to chamber 4) along the route: 3 → 2 → 4
This path has 2 corridors (3–2, 2–4), so the length is 2.

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
  public static void main(String args[]) {
    Scanner sc = new Scanner(System.in);

    int vals[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();

    System.out.println(longestDistance(vals));

    sc.close();
  }

  private static int longestDistance(int[] vals) {
    Node root = buildTree(vals);
    
    return secretRouteDistance(root);
  }

  private static Node buildTree(int[] vals) {
    if (vals.length == 0)
      return null;

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

  private static int helper(Node root, int[] res) {
    if (root==null) return 0;
    
    int lh = helper(root.l, res), rh = helper(root.r, res);
    res[0] = Math.max(lh + rh, res[0]);
    return Math.max(lh, rh) + 1;
  }

  private static int secretRouteDistance(Node root) {
    if (root==null) return 0;
    
    int[] res = { 0 };
    int ld=helper(root.l, res), rd=helper(root.r, res);
    
    return Math.max(res[0], ld + rd);
  }
}
```