#Coding/Trees/Reconstruction #Coding/Trees/Traversal #Coding/BFS 

Given the preorder and postorder traversals of a full binary tree, construct 
The original binary tree and print its level order traversal.

Input Format:
---------------
Space separated integers, pre order data
Space separated integers, post order data

Output Format:
-----------------
Print the level-order data of the tree.


Sample Input-1:
----------------
```
1 2 4 5 3 6 7
4 5 2 6 7 3 1
```

Sample Output-1:
----------------
```
[[1], [2, 3], [4, 5, 6, 7]]
```

Explanation:
--------------
```
        1
       / \
      2   3
     / \  / \
    4  5  6  7
```


Sample Input-2:
----------------
```
1 2 3
2 3 1
```

Sample Output-2:
----------------
```
[[1], [2, 3]]
```

Explanation:
--------------
```
    1
   / \
  2   3
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

    int[] preOrder = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    int[] postOrder = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    
    System.out.println(levels(preOrder, postOrder));
    
    sc.close();
  }
    
  private static List<List<Integer>> levels(int[] preOrder, int[] postOrder) {
    Map<Integer, Integer> postOrderIdxs = new HashMap<>();
    
    for (int i=0; i<postOrder.length; i++) postOrderIdxs.put(postOrder[i], i);

    Node tree = buildTree(preOrder, postOrderIdxs, 0, preOrder.length-1, 0);
    
    return levelOrder(tree);
  }
    
  private static Node buildTree(int[] preOrder, Map<Integer, Integer> postOrderIdxs, int l, int r, int p) {
    if (l>r) return null;
    
    Node root = new Node(preOrder[l]);
    
    if (l==r) return root;
    
    int idx = postOrderIdxs.get(preOrder[l+1]);
    int lsize = idx-p+1;
    
    root.l = buildTree(preOrder, postOrderIdxs, l+1, l+lsize, p);
    root.r = buildTree(preOrder, postOrderIdxs, l+lsize+1, r, idx+1);
    
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