In an Intelligence Agency, each senior officer supervises either two junior officers 
or none. The senior officer is assigned a clearance level equal to the higher clearance 
level of the two junior officers they supervise.

The clearance levels are represented as integer values in the range \[1, 50], and multiple 
officers may have the same clearance level.

At the end, all officers (senior and junior) are collectively referred to as agents in the system.

You are provided with a hierarchical clearance level tree where each node represents 
an officer's clearance level. The tree structure follows these rules:
 - If a node has two children, its clearance level is the maximum of the two children's clearance levels.
 - If a node has no children, it's clearance level is same as exists.
 - The value -1 indicates an empty (null) position.

Given the hierarchy of the agency, your task is to find the second highest clearance level among all agents in the agency. If no such level exists, return -2.

Input Format:
-------------
A single line of space separated integers, clearance levels of each individual.

Output Format:
--------------
Print an integer, second top agent based on rank.

Sample Input-1:
---------------
```
5 5 4 -1 -1 2 4
```

Sample Output-1:
----------------
```
4
```

Sample Input-2:
---------------
```
3 3 3 3 3
```

Sample Output-2:
----------------
```
-2
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

    System.out.println(getSecondMax(vals));

    sc.close();
  }

  private static int getSecondMax(int[] vals) {
    Node root = buildTree(vals);

    return secondMax(root);
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

  private static int secondMax(Node root) {
    if (root==null) return -1;
    
    int l = -2, r = -2; 
    if (root.l!=null) l=root.l.val;
    if (root.r!=null) r=root.r.val;
    
    if (l==root.val) l=secondMax(root.l);
    if (r==root.val) r=secondMax(root.r);
    
    int res = Math.max(l, r);	
    return res==root.val ? -2 : res;
  }
}
```