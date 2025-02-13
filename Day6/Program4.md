#Coding/Trees/Construction #Coding/Trees/Traversal 

The Indian Army has established multiple military camps at strategic locations 
Along the Line of Actual Control (LAC) in Galwan. These camps are connected in 
A hierarchical structure, with a main base camp acting as the root of the network.

To fortify national security, the Government of India has planned to deploy 
a protective S.H.I.E.L.D. that encloses all military camps forming the outer 
Boundary of the network.

Structure of Military Camps:
    - Each military camp is uniquely identified by an integer ID.
    - A camp can have at most two direct connections:
        - Left connection → Represents a subordinate camp on the left.
        - Right connection → Represents a subordinate camp on the right.
    - If a military camp does not exist at a specific position, it is 
      Represented by -1
	
Mission: Deploying the S.H.I.E.L.D.
Your task is to determine the list of military camp IDs forming the outer 
Boundary of the military network. This boundary must be traversed in 
Anti-clockwise order, starting from the main base camp (root).

The boundary consists of:
1. The main base camp (root).
2. The left boundary:
    - Starts from the root’s left child and follows the leftmost path downwards.
    - If a camp has a left connection, follow it.
    - If no left connection exists but a right connection does, follow the right connection.
    - The leftmost leaf camp is NOT included in this boundary.
3. The leaf camps (i.e., camps with no further connections), ordered from left to right.
4. The right boundary (in reverse order):
    - Starts from the root’s right child and follows the rightmost path downwards.
    - If a camp has a right connection, follow it.
    - If no right connection exists but a left connection does, follow the left connection.
    - The rightmost leaf camp is NOT included in this boundary.


Input Format:
-------------
Space separated integers, military-camp IDs.

Output Format:
--------------
Print all the military-camp IDs, which are at the edge of S.H.I.E.L.D.


Sample Input-1:
---------------
```
5 2 4 7 9 8 1
```

Sample Output-1:
----------------
```
[5, 2, 7, 9, 8, 1, 4]
```


Sample Input-2:
---------------
```
11 2 13 4 25 6 -1 -1 -1 7 18 9 10
```

Sample Output-2:
----------------
``[11, 2, 4, 7, 18, 9, 10, 6, 13]``


Sample Input-3:
---------------
```
1 2 3 -1 -1 -1 5 -1 6 7
```

Sample Output-3:
----------------
```
[1, 2, 7, 6, 5, 3]
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

    System.out.println(boundary(vals));

    sc.close();
  }

  public static List<Integer> boundary(int[] vals) {
    Node root = buildTree(vals);

    List<Integer> res = new LinkedList<>() {{ add(root.val); }};
    left(root.l, res);
    leaves(root, res);
    right(root.r, res);
    return res;
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
    
  private static void left(Node root, List<Integer> res) {
    if (root==null || (root.l==null && root.r==null)) return;

    res.add(root.val);

    if (root.l!=null) left(root.l, res);
    else left(root.r, res);
  }
  
  private static void leaves(Node root, List<Integer> res) {
    if (root==null) return;

    if (root.l==null && root.r==null) res.add(root.val);

    if (root.l!=null) leaves(root.l, res);
    if (root.r!=null) leaves(root.r, res);
  }
  
  private static void right(Node root, List<Integer> res) {
    if (root==null || (root.l==null && root.r==null)) return;

    if (root.r!=null) right(root.r, res);
    else right(root.l, res);

    res.add(root.val);
  }
}
```