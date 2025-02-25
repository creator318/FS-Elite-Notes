#Coding/Trees/Symmetry

Mr. Rakesh is interested in working with Data Structures.

He has constructed a Binary Tree (BT) and asked his friend
Anil to check whether the BT is a self-mirror tree or not.

Can you help Rakesh determine whether the given BT is a self-mirror tree?
Return true if it is a self-mirror tree; otherwise, return false.

Note:
------
In the tree, '-1' indicates an empty (null) node.

Input Format:
-------------
A single line of space separated integers, values at the treenode

Output Format:
--------------
Print a boolean value.

Sample Input-1:
---------------
```
2 1 1 2 3 3 2
```

Sample Output-1:
----------------
```
true
```

Sample Input-2:
---------------
```
2 1 1 -1 3 -1 3
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

    System.out.println(isSymmetric(vals));

    sc.close();
  }
    
  private static boolean isSymmetric(int[] vals) {
    Node root = buildTree(vals);

    return symmetric(root.l, root.r);
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

  private static boolean symmetric(Node l, Node r) {
    if (l==null && r==null) return true;
    if (l==null || r==null || l.val!=r.val) return false;
    
    return symmetric(l.l, r.r) && symmetric(l.r, r.l);
  }
}
```