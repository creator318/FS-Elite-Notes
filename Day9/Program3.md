#Coding/Trees/Traversal 

Imagine you are a librarian organizing books on vertical shelves in a grand library. The books are currently scattered across a tree-like structure, where each book (node) has a position determined by its shelf number (column) and row number (level).

Your task is to arrange the books on shelves so that:
1. Books are placed column by column from left to right.
2. Within the same column, books are arranged from top to bottom (i.e., by row).
3. If multiple books belong to the same shelf and row, they should be arranged from left to right, just as they appear in the original scattered arrangement.

Sample Input-1:
-------------
```
3 9 20 -1 -1 15 7
```

Sample Output-1:
--------------
```
[[9],[3,15],[20],[7]]
```

Explanation:
------------
```
  3
 / \
9   20
    / \
   15  7
```

Shelf 1: \[9]
Shelf 2: \[3, 15]
Shelf 3: \[20]
Shelf 4: \[7]

Sample Input-2:
---------------
```
3 9 8 4 0 1 7
```

Sample Output-2:
----------------
```
[[4],[9],[3,0,1],[8],[7]]
```

Explanation:
------------
```
    3
   / \
  9   8
 / \ / \
4  0 1  7
```

Shelf 1: \[4]
Shelf 2: \[9]
Shelf 3: \[3, 0, 1]
Shelf 4: \[8]
Shelf 5: \[7]

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

class NodeShelf {
  Node node;
  int col;
  NodeShelf(Node node, int col) {
    this.node = node;
    this.col = col;
  }
}

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    int[] vals = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();

    System.out.println(columns(vals));

    sc.close();
  }

  private static List<List<Integer>> columns(int[] vals) {
    Node root = buildTree(vals);

    List<List<Integer>> res = new LinkedList<>(vertical(root).values());
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

  private static Map<Integer, List<Integer>> vertical(Node root) {
    Map<Integer, List<Integer>> res = new TreeMap<>();
    Queue<NodeShelf> q = new LinkedList<>() {{ add(new NodeShelf(root, 0)); }};

    while (!q.isEmpty()) {
      NodeShelf curr = q.remove();

      if (!res.containsKey(curr.col)) res.put(curr.col, new LinkedList<>());
      res.get(curr.col).add(curr.node.val);

      if (curr.node.l!=null) q.add(new NodeShelf(curr.node.l, curr.col-1));
      if (curr.node.r!=null) q.add(new NodeShelf(curr.node.r, curr.col+1));
    }
    return res;
  }
}
```