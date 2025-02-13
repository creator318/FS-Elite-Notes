#Coding/Trees/Reconstruction #Coding/Trees/Traversal  #Coding/BFS 

Construct the binary tree from the given In-order and Pre-order. 
Find Nodes Between Two Levels in Spiral Order.
The spiral order is as follows:
-> Odd levels → Left to Right.
-> Even levels → Right to Left.

Input Format:
--------------
An integer N representing the number of nodes.
A space-separated list of N integers representing the in-order traversal.
A space-separated list of N integers representing the pre-order traversal.
Two integers:
- Lower Level (L)
- Upper Level (U)

Output Format:
--------------
Print all nodes within the specified levels, but in spiral order.

Sample Input-1:
----------
```
7
4 2 5 1 6 3 7
1 2 4 5 3 6 7
2 3
```

Sample Output-1:
----------
```
3 2 4 5 6 7
```

Explanation:
----------
Binary tree structure:
```
        1
       / \
      2   3
     / \  / \
    4  5  6  7
```

Levels 2 to 3 in Regular Order:
Level 2 → 2 3
Level 3 → 4 5 6 7

Spiral Order:
Level 2 (Even) → 3 2 (Right to Left)
Level 3 (Odd) → 4 5 6 7 (Left to Right)

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

    int n = sc.nextInt();
    int[] inOrder = new int[n];
    for (int i=0; i<n; i++) inOrder[i] = sc.nextInt();
    int[] preOrder = new int[n];
    for (int i=0; i<n; i++) preOrder[i] = sc.nextInt();

    int fl = sc.nextInt(), ll = sc.nextInt();

    System.out.println(levels(inOrder, preOrder, fl, ll));

    sc.close();
  }

  private static List<Integer> levels(int[] inOrder, int[] preOrder, int fl, int ll) {
    Map<Integer, Integer> inOrderIdxs = new HashMap<>();
    for (int i=0; i<inOrder.length; i++) inOrderIdxs.put(inOrder[i], i);

    int[] p = {0}; // Share reference :|

    Node tree = buildTree(inOrderIdxs, preOrder, 0, inOrder.length - 1, p);

    List<List<Integer>> levels = levelOrder(tree);

    List<Integer> res = new LinkedList<>();
    for (int i=fl-1; i<ll; i++) res.addAll(levels.get(i));
    return res;
  }

  private static Node buildTree(Map<Integer, Integer> inOrderIdxs, int[] preOrder, int l, int r, int[] p) {
    if (l>r) return null;

    Node root = new Node(preOrder[p[0]]);
    int idx = inOrderIdxs.get(preOrder[p[0]]);
    p[0]++;

    root.l = buildTree(inOrderIdxs, preOrder, l, idx - 1, p);
    root.r = buildTree(inOrderIdxs, preOrder, idx + 1, r, p);

    return root;
  }

  private static List<List<Integer>> levelOrder(Node root) {
    List<List<Integer>> res = new LinkedList<>();
    Queue<Node> q = new LinkedList<>() {{ add(root); }}; 

    while (!q.isEmpty()) {
      int size = q.size();
      List<Integer> currLvl = new LinkedList<>();

      for (int i=0; i<size; i++) {
        Node currNode = q.remove();
        currLvl.add(currNode.val);
        if (currNode.l!=null) q.add(currNode.l);
        if (currNode.r!=null) q.add(currNode.r);
      }
      
      if (res.size()%2 == 1) Collections.reverse(currLvl);
      res.add(currLvl);
    }

    return res;
  }
}
```