#Coding/Trees/Traversal #Coding/BFS  

Imagine you are navigating a maze where each path you take has a section with a 
code. The maze is structured as a series of interconnected rooms, 
represented as a tree structure. Each room in the maze has a code (integral value)
associated with it, and you are trying to check if a given sequence of code 
matches a valid path from the entrance to an exit. 

You are provide with the maze structure, where you have to build the maze and then,
you are provided with a series of space seperated digits, representing a journey 
starting from the entrance and passing through the rooms along the way. 
The task is to verify whether the path corresponding to this array of codes 
exists in the maze.

Sample Input-1:
------
```
0 1 0 0 1 0 -1 -1 1 0 0
0 1 0 1
```

Sample Output-1: 
------
```
true
```

Explanation:
------
```
     0
   /   \
  1     0
 / \   /
0   1  0
 \ / \
 1 0  0
```
The given path 0 → 1 → 0 → 1 is a valid path in the maze. 
Other valid sequences in the maze include 0 → 1 → 1 → 0 and 0 → 0 → 0.


Sample Input-2:
------
```
1 2 3
1 2 3
```

Sample Output-2:
------
```
false
```

Explanation:
------
```
  1
 / \
2   3
```
The proposed path 1 → 2 → 3 does not exist in the maze, 
so it cannot be a valid path.

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
    int[] path = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
  
    System.out.println(pathExists(vals, path));

    sc.close();
  }
    
  private static boolean pathExists(int[] vals, int[] path) {
    Node root = buildTree(vals);

    return searchPath(root, path, 0);
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

  private static boolean searchPath(Node root, int[] path, int i) {
    if (i==path.length) return true;
    
    if (root==null) return false;
    if (root.val != path[i]) return false;
    
    return searchPath(root.l, path, i+1) || searchPath(root.r, path, i+1);
  }
}
```