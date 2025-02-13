#Coding/Trees/Reconstruction #Coding/Trees/Traversal  #Coding/BFS 

Given the in-order and post-order traversals of a binary tree, construct 
The original binary tree. For the given Q number of queries, 
Each query consists of a lower level and an upper level. 
The output should list the nodes in the order they appear in a level-wise.

Input Format:
-------------
An integer N representing the number nodes.
A space-separated list of N integers representing the similar to in-order traversal.
A space-separated list of N integers representing the similar to post-order traversal.
An integer Q representing the number of queries.
Q pairs of integers, each representing a query in the form:
- Lower level (L)
- Upper level (U)

Output Format:
---------------
For each query, print the nodes in order within the given depth range

Sample Input-1:
----------
```
7
4 2 5 1 6 3 7
4 5 2 6 7 3 1
2
1 2
2 3
```

Sample Output-1:
----------
```
[1, 2, 3]
[2, 3, 4, 5, 6, 7]
```

Explanation:
----------
```
        1
       / \
      2   3
     / \  / \
    4  5  6  7
```
Query 1 (Levels 1 to 2): 1 2 3
Query 2 (Levels 2 to 3): 2 3 4 5 6 7

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
    int[] postOrder = new int[n];
    for (int i=0; i<n; i++) postOrder[i] = sc.nextInt();

    int q = sc.nextInt();
    int[][] queries = new int[q][2];
    for (int i=0; i<q; i++) {
      queries[i][0] = sc.nextInt();
      queries[i][1] = sc.nextInt();
    }

    for (List<Integer> lvls : levels(inOrder, postOrder, queries))
      System.out.println(lvls);

    sc.close();
  }

  private static List<List<Integer>> levels(int[] inOrder, int[] postOrder, int[][] queries) {
    Map<Integer, Integer> inOrderIdxs = new HashMap<>();
    for (int i=0; i<inOrder.length; i++) inOrderIdxs.put(inOrder[i], i);

    int[] p = {postOrder.length-1}; // Share reference :|

    Node tree = buildTree(inOrderIdxs, postOrder, 0, inOrder.length - 1, p);

    List<List<Integer>> levels = levelOrder(tree);
    
    List<List<Integer>> res = new LinkedList<>();
    for (int[] q: queries) {
      List<Integer> curr = new LinkedList<>();
      for (int i=q[0]-1; i<q[1]; i++) curr.addAll(levels.get(i));
      res.add(curr);
    }
    return res;
  }

  private static Node buildTree(Map<Integer, Integer> inOrderIdxs, int[] postOrder, int l, int r, int[] p) {
    if (l>r) return null;

    Node root = new Node(postOrder[p[0]]);
    int idx = inOrderIdxs.get(postOrder[p[0]]);
    p[0]--;

    root.r = buildTree(inOrderIdxs, postOrder, idx + 1, r, p);
    root.l = buildTree(inOrderIdxs, postOrder, l, idx - 1, p);

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
      res.add(currLvl);
    }

    return res;
  }
}
```