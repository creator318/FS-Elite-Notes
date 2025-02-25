#Coding/Trees/Splitting  

Balbir Singh is working with networked systems, where servers are connected
in a hierarchical structure, represented as a Binary Tree. Each server (node)
has a certain processing load (integer value).

Balbir has been given a critical task: split the network into two balanced
sub-networks by removing only one connection (edge). The total processing
load in both resulting sub-networks must be equal after the split.

Network Structure:
- The network of servers follows a binary tree structure.
- Each server is represented by an integer value, indicating its processing
load.
- If a server does not exist at a particular position, it is represented as '-1' (NULL).

Help Balbir Singh determine if it is possible to split the network into two equal processing load sub-networks by removing exactly one connection.

Input Format:
-------------
Space separated integers, elements of the tree.

Output Format:
--------------
Print a boolean value.

Sample Input-1:
---------------
```
1 2 3 5 -1 -1 5
```

Sample Output-1:
----------------
```
true
```

Sample Input-2:
---------------
```
3 2 4 3 2 -1 7
```

Sample Output-2:
----------------
```
false
```

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

    System.out.println(canSplitEqual(vals));

    sc.close();
  }
  
  private static boolean canSplitEqual(int[] vals) {
    Node root = buildTree(vals);
    
    return isEqual(root, 0);
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

  private static int sum(Node root) {
    if (root == null) return 0;

    return root.val + sum(root.l) + sum(root.r);
  }
  
  private static boolean isEqual(Node root, int sum) {
    if (root==null) return false;
    
    int lsum = sum(root.l), rsum = sum(root.r);
    
    if (lsum<rsum) {
      if (rsum==lsum+root.val+sum) return true;
      return isEqual(root.r, lsum+root.val+sum);
    } else if (lsum>rsum) {
      if (lsum==rsum+root.val+sum) return true;
      return isEqual(root.l, rsum+root.val+sum);
    } else return false;
  }
}
```