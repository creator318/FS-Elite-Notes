#Coding/Trees/Traversal  

Imagine you're the chief curator at a renowned museum that houses a rare collection of ancient artifacts. 
These artifacts are arranged in a special display that follows a strict rule: any artifact positioned to the left of another has a lower value, and any artifact on the right has a higher value. 

Your task is to transform this display into what is known as a "Greater Artifact Display." In this new arrangement, each artifact’s new value will be its original value plus the sum of the values of all artifacts that are equal ormore valuable than it.

Sample Input-1:
----------
```
4 2 6 1 3 5 7
```

Sample Output-1:
----------
```
22 27 13 28 25 18 7
```

Explanation:
----------
Input structure 
```
       4
      / \
     2   6
    / \ / \
   1  3 5  7
```

Output structure:
```
       22
      /  \
     27  13
    / \  / \
  28 25  18 7
```

Reverse updates:
- Artifact 7: 7
- Artifact 6: 6 + 7 = 13
- Artifact 5: 5 + 13 = 18
- Artifact 4: 4 + 18 = 22
- Artifact 3: 3 + 22 = 25
- Artifact 2: 2 + 25 = 27
- Artifact 1: 1 + 27 = 28

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

    System.out.println(greaterArtifacts(vals));

    sc.close();
  }

  private static List<Integer> greaterArtifacts(int[] vals) {
    Node root = buildBSTree(vals);

    int max[] = {0};
    getGreaterArtifacts(root, max);

    return levelOrder(root);
  }

  private static Node buildBSTree(int[] vals) {
    if (vals.length == 0) return null;
    
    Node root = new Node(vals[0]);

    for (int i=1; i<vals.length; i++) insert(root, vals[i]);
    
    return root;
  }

  private static Node insert(Node root, int val) {
    if (root==null) return new Node(val);

    if (val<root.val) root.l = insert(root.l, val);
    if (val>root.val) root.r = insert(root.r, val);
    
    return root;
  }
  
  private static void getGreaterArtifacts(Node root, int[] max) {
    if (root==null) return;

    getGreaterArtifacts(root.r, max);
    root.val += max[0];
    max[0] = root.val;
    getGreaterArtifacts(root.l, max);
  }

  private static List<Integer> levelOrder(Node root) {
    List<Integer> res = new LinkedList<>();
    Queue<Node> q = new LinkedList<>() {{ add(root); }};

    while (!q.isEmpty()) {
      int size = q.size();
      for (int i=0; i<size; i++) {
        Node curr = q.remove();
        res.add(curr.val);
        if (curr.l != null)
          q.add(curr.l);
        if (curr.r != null)
          q.add(curr.r);
      }
    }

    return res;
  }
}
```