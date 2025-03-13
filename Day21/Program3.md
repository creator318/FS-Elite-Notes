#Coding/Trees/Traversal 

Imagine you're the curator of an ancient manuscript archive. Each manuscript is assigned a unique significance score, and the archive is arranged so that every manuscript on the left has a lower score and every manuscript on the right has a higher score, forming a special ordered display. Now, for an upcoming exhibition, scholars have decided that only manuscripts with significance scores between low and high (inclusive) are relevant. Your task is to update the archive by removing any manuscripts whose scores fall outside the range \[low, high]. Importantly, while you remove some manuscripts, you must preserve the relative order of the remaining ones—if a manuscript was originally displayed as a descendant of another, that relationship should remain intact. It can be proven that there is a unique way to update the archive.

Display the manuscript of the updated archive. Note that the main manuscript (the root) may change depending on the given score boundaries.

Input format:
----------
Line 1: space separated scores to build the manuscript archive
Line 2: two space seperated integers, low and high.

Output format:
----------
Space separated scores of the updated archive

Sample Input-1:
----------
```
1 0 2
1 2
```

Sample Output-1:
```
1 2
```

Explanation:
----------
Initial archive:
```
      1
     / \
    0   2
```

Updated archive:
```
    1
     \
      2
```

After removing manuscripts scores below 1 (i.e. 0), only the manuscript with 1 
and its right manuscript 2 remain.

Sample Input-2:
----------
```
3 0 4 2 1
1 3
```

Sample Output-2:
----------
```
3 2 1
```

Explanation:
----------
Initial archive:
```
          3
         / \
        0   4
         \
          2
         /
        1
```

Updated archive:
```
      3
     /
    2
   /
  1
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

    int vals[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    int l = sc.nextInt(), h = sc.nextInt();

    System.out.println(filterManuscripts(vals, l, h));

    sc.close();
  }

  private static List<Integer> filterManuscripts(int[] vals, int l, int h) {
    Node root = buildBSTree(vals);

    root = filterTree(root, l, h);

    return levelOrder(root);
  }

  private static Node buildBSTree(int[] vals) {
    if (vals.length==0) return null;
    
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

  private static Node filterTree(Node root, int l, int h) {
    if (root==null) return null;

    if (root.val<l) return filterTree(root.r, l, h);
    if (root.val>h) return filterTree(root.l, l, h);

    root.l = filterTree(root.l, l, h);
    root.r = filterTree(root.r, l, h);

    return root;
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