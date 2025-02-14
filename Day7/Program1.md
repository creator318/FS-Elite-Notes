#Coding/Trees/LeastCommonAncestor

Bubloo is working with computer networks, where servers are connected 
in a hierarchical structure, represented as a Binary Tree. Each server (node) 
is uniquely identified by an integer value.

Bubloo has been assigned an important task: find the shortest communication 
path (in terms of network hops) between two specific servers in the network.

Network Structure:
------------------
The network of servers follows a binary tree topology.
Each server (node) has a unique identifier (integer).
If a server does not exist at a certain position, it is represented as '-1' (NULL).

Given the root of the network tree, and two specific server IDs (E 1 & E 2), 
determine the minimum number of network hops (edges) required to 
communicate between these two servers.

Input Format:
-------------
Space separated integers, elements of the tree.

Output Format:
--------------
Print an integer value.


Sample Input-1:
---------------
```
1 2 4 3 5 6 7 8 9 10 11 12
4 8
```

Sample Output-1:
----------------
```
4
```

Explanation:
------------
The edegs between 4 and 8 are: \[4,1], \[1,2], \[2,3], \[3,8]


Sample Input-2:
---------------
```
1 2 4 3 5 6 7 8 9 10 11 12
6 6
```

Sample Output-2:
----------------
```
0
```

Explanation:
----------------
No edges between 6 and 6

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
    int a = sc.nextInt(), b = sc.nextInt();

    System.out.println(leastDistance(vals, a, b));

    sc.close();
  }
    
  private static int leastDistance(int[] vals, int a, int b) {
    Node root = buildTree(vals);

    Node LCA = leastCommonAncestor(root, a, b);
    return distance(LCA, a, 0) + distance(LCA, b, 0);
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

  private static Node leastCommonAncestor(Node root, int a, int b) {
    if (root == null || root.val == a || root.val == b) return root;

    Node l = leastCommonAncestor(root.l, a, b), r = leastCommonAncestor(root.r, a, b);
   
    if (l!=null && r!=null) return root;

    if (l!=null) return l;
    else return r;
  }
  
  private static int distance(Node root, int t, int d) {
    if (root==null) return 0;
    if (root.val==t) return d;
    
    int l=distance(root.l, t, d+1), r=distance(root.r, t, d+1);
    return Math.max(l, r);
  }
}
```