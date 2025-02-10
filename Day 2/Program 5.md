#Coding/LevelOrder #Coding/InOrder #Coding/BFS

Write a program to construct a binary tree from level-order input, while treating -1 
As a placeholder for missing nodes. The program reads input, constructs the tree, 
And provides an in-order traversal to verify correctness.

Input Format:
---------------
Space separated integers, level order data (where -1 indiactes null node).

Output Format:
-----------------
Print the in-order data of the tree.


Sample Input-1:
----------------
```
1 2 3 -1 -1 4 5
```

Sample Output-1:
----------------
```
2 1 4 3 5
```

Explanation:
--------------
 ```
      1
     / \
    2   3
       / \
      4   5
```

Sample Input-2:
----------------
```
1 2 3 4 5 6 7
```

Sample Output-2:
----------------
```
4 2 5 1 6 3 7
```

Explanation:
--------------
```
        1
       / \
      2   3
     / \ / \
    4  5 6  7
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

class Tree {
    static Node root = null;
    
    Tree(int[] vals) {
        if (vals.length == 0) return;
        
        root = new Node(vals[0]);
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
    }
        
    public List<Integer> inOrder() {
        List<Integer> res = new LinkedList<>();
        inOrder(root, res);
        return res;
    }
    
    private void inOrder(Node root, List<Integer> res) {
        if (root==null) return;
        
        inOrder(root.l, res);
        res.add(root.val);
        inOrder(root.r, res);
    } 
}

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int[] vals = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();

        Tree t = new Tree(vals);
        List<Integer> inOrder = t.inOrder();
        
        for (int i: inOrder) System.out.print(i + " ");
        System.out.println();

        sc.close();
    }
}
```