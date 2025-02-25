#Coding/Trees/Transformation 

You are a gardener designing a beautiful floral pathway in a vast botanical 
Garden. The garden is currently overgrown with plants, trees, and bushes 
Arranged in a complex branching structure, much like a binary tree. Your task 
Is to carefully prune and rearrange the plants to form a single-file walking 
Path that visitors can follow effortlessly.

To accomplish this, you must flatten the garden’s layout into a linear sequence 
While following these rules:
 1. The garden path should maintain the same PlantNode structure, where the right branch connects to the next plant in the sequence, and the left branch is always trimmed (set to null).
 2. The plants in the final garden path should follow the same arrangement as a pre-order traversal of the original garden layout. 

Input Format:
-------------
Space separated integers, elements of the tree.

Output Format:
--------------
Print the list.

Sample Input-1:
-------------
```
1 2 5 3 4 -1 6
```

Sample Output-1:
--------------
```
1 2 3 4 5 6
```

Explanation:
------------
Input structure:
```
       1
      / \
     2   5
    / \    \
   3   4    6
```

Output structure:
```
	1
	 \
	  2
	   \
      3
	  	 \
        4
         \
          5
           \
            6
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
  public static void main(String args[]) {
    Scanner sc = new Scanner(System.in);

    int vals[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();

    System.out.println(treeToList(vals));
    
    sc.close();
  }

  private static List<Integer> treeToList(int[] vals) {
    Node root = buildTree(vals);

    root = convertToList(root);

    List<Integer> res = new LinkedList<>();
    Node curr = root;
    while (curr != null) {
      res.add(curr.val);
      curr = curr.r;
    }
    
    return res;
  }

  private static Node buildTree(int[] vals) {
    if (vals.length == 0)
      return null;

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
  
  private static Node convertToList(Node root) {
    Stack<Node> stk = new Stack<>() {{ push(root); }};
    
    while (!stk.isEmpty()) {
      Node curr = stk.pop();

      if (curr.r!=null) stk.push(curr.r);
      if (curr.l!=null) stk.push(curr.l);
      
      curr.r = stk.isEmpty() ? null : stk.peek();
      curr.l = null;
    }
    return root;
  }
}
```