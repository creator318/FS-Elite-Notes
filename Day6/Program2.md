#Coding/Trees/Traversal #Coding/BFS #Coding/DFS 

Balbir Singh is working with Binary Trees.
The elements of the tree are given in level-order format.

Balbir is observing the tree from the right side, meaning he 
Can only see the rightmost nodes (one node per level).

You are given the root of a binary tree. Your task is to determine 
The nodes visible from the right side and return them in top-to-bottom order.

Input Format:
-------------
Space separated integers, elements of the tree.

Output Format:
--------------
A list of integers representing the node values visible from the right side


Sample Input-1:
---------------
```
1 2 3 5 -1 -1 5
```

Sample Output-1:
----------------
```
[1, 3, 5]
```



Sample Input-2:
---------------
```
3 2 4 3 2
```

Sample Output-2:
----------------
```
[3, 4, 2]
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

    System.out.println(rightView(vals));

    sc.close();
  }
    
  private static List<Integer> rightView(int[] vals) {
    Node root = buildTree(vals);
    
    return levelOrder(root);
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
    
  private static List<Integer> levelOrder(Node root) {
    List<Integer> res = new LinkedList<>();
    Queue<Node> q = new LinkedList<>() {{ add(root); }};

    while (!q.isEmpty()) {
      int size = q.size(), currRight=0;
      for (int i=0; i<size; i++) {
        Node currNode = q.remove();
        currRight = currNode.val;
        if (currNode.l!=null) q.add(currNode.l);
        if (currNode.r!=null) q.add(currNode.r);
      }
      res.add(currRight);
    }

    return res;
  }
}
```