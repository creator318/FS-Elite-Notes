#Coding/Trees/Traversal #Coding/Trees/Transformation

VishnuVardan is working with Decision Trees for AI-based predictions.
To analyze alternative outcomes, Kishore has planned to flip the decision 
Tree horizontally to simulate a reverse processing approach.

Rules for Flipping the Decision Tree:
	- The original root node becomes the new rightmost node.
	- The original left child becomes the new root node.
	- The original right child becomes the new left child.
This transformation is applied level by level recursively.

Note:
------
- Each node in the given tree has either 0 or 2 children.
- Every right node in the tree has a left sibling sharing the same parent.
- Every right node has no further children (i.e., they are leaf nodes).

Your task is to help VishnuVardan flip the Decision Tree while following 
The given transformation rules.

Input Format:
-------------
Space separated integers, nodes of the tree.

Output Format:
--------------
Print the list of nodes of flipped tree as described below.


Sample Input-1:
---------------
```
4 2 3 5 1
```

Sample Output-1:
----------------
```
5 1 2 3 4
```


Sample Input-2:
---------------
```
4 2 5
```

Sample Output-2:
----------------
```
2 5 4
```

## Solution

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

    System.out.println(flip(vals));

    sc.close();
  }

  private static List<Integer> flip(int[] vals) {
    Node root = buildTree(vals);

    root = flipTree(root);

    return levelOrder(root);
  }

  private static Node buildTree(int[] vals) {
    if (vals.length == 0)
      return null;

    Node root = new Node(vals[0]);
    Queue<Node> q = new LinkedList<>();
    q.add(root);

    int i = 0;
    while (i < vals.length - 1) {
      Node curr = q.remove();

      curr.l = new Node(vals[++i]);
      q.add(curr.l);

      curr.r = new Node(vals[++i]);
    }

    return root;
  }

  private static Node flipTree(Node root) {
    if (root == null || root.l == null)
      return root;

    Node res = flipTree(root.l);

    root.l.l = root.r;
    root.l.r = root;
    root.l = root.r = null;

    return res;
  }

  private static List<Integer> levelOrder(Node root) {
    List<Integer> res = new LinkedList<>() {{ add(root.val); }};
    Queue<Node> q = new LinkedList<>() {{ add(root); }};

    while (!q.isEmpty()) {
      Node curr = q.remove();
      if (curr.l != null)
        res.add(curr.l.val);
      if (curr.r != null) {
        q.add(curr.r);
        res.add(curr.r.val);
      }
    }

    return res;
  }
}
```