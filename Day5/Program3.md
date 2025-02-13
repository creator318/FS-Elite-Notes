#Coding/Trees/Reconstruction #Coding/BFS

Construct Tree from the given Level-order and In-order.
Compute the Depth and Sum of the Deepest nodes in the Binary tree

Input Format:
-------------
An integer N representing the number of nodes.
A space-separated list of N integers representing the in-order traversal.
A space-separated list of N integers representing the level-order traversal.

Output Format:
--------------
Print two values:
->The maximum number of levels.
->The sum of all node values at the deepest level.

Sample Input-1:
----------
```
11
7 8 4 2 5 9 11 10 1 6 3
1 2 3 4 5 6 7 9 8 10 11
```

Sample Output-1:
----------
```
6 11
```

Explanation:
----------
The binary tree structure:
```
           1
         /   \
       2       3
      / \     /
     4   5   6
    /     \   
   7       9
    \       \
     8      10
            /
          11
```
Maximum Depth: 6
Nodes at the Deepest Level (6): 11
Sum of nodes at Level 6: 11

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
    int[] lvlOrder = new int[n];
    for (int i=0; i<n; i++) lvlOrder[i] = sc.nextInt();

    int[] res = solve(inOrder, lvlOrder);
    
    System.out.println(res[0] + " " + res[1]);
    
    sc.close();
  }
  
  private static int[] solve(int[] inOrder, int[] lvlOrder) {
    Map<Integer, Integer> lvlOrderIdxs = new HashMap<>();
    for (int i = 0; i < inOrder.length; i++) lvlOrderIdxs.put(lvlOrder[i], i);

    Node tree = builTree(inOrder, lvlOrderIdxs, 0, lvlOrder.length - 1);
    
    return new int[] {maxDepth(tree), deepestLevelSum(tree)};
  }

  private static Node builTree(int[] inOrder, Map<Integer, Integer> lvlOrderIdxs, int l, int r) {
    if (l>r) return null;

    int idx = findFirst(inOrder, lvlOrderIdxs, l, r);
    Node root = new Node(inOrder[idx]);

    root.l = builTree(inOrder, lvlOrderIdxs, l, idx-1);
    root.r = builTree(inOrder, lvlOrderIdxs, idx+1, r);

    return root;
  }
  
  private static int findFirst(int[] inOrder, Map<Integer, Integer> lvlOrderIdxs, int l, int r) {
    int min = Integer.MAX_VALUE, idx = -1;
    for (int i=l; i<=r; i++) {
      int curr = lvlOrderIdxs.get(inOrder[i]);
      if (curr<min) idx = i;
      min = Math.min(min, curr);
    }
    return idx;
  }

  private static int maxDepth(Node root) {
    if (root==null) return 0;
    return Math.max(maxDepth(root.l), maxDepth(root.r)) + 1;
  }
  
  private static int deepestLevelSum(Node root) {
    int sum = 0;
    Queue<Node> q = new LinkedList<>() {{ add(root); }};

    while (!q.isEmpty()) {
      int size = q.size();
      sum = 0;
      for (int i=0; i<size; i++) {
        Node curr = q.remove();
        sum += curr.val;
        if (curr.l!=null) q.add(curr.l);
        if (curr.r!=null) q.add(curr.r);
      }
    }
    return sum;
  }
}
```