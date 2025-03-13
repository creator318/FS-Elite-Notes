#Coding/Trees/Traversal

Imagine you are designing a grand castle where each room holds a specific amount of treasure. The castle is built in a binary layout, meaning every room may lead to two adjacent wings—a left wing and a right wing. 

An "organized section" of the castle follows this rule: for any given room, every room in its left wing contains a treasure value that is strictly less than the current room’s value, and every room in its right wing contains a value that is strictly greater. Additionally, each wing must itself be organizedaccording to the same rule.

Your challenge is to determine the maximum total treasure (i.e., the sum of treasure values) that can be gathered from any such organized section of the castle.

Sample Imput-1:
----------
```
1 4 3 2 4 2 5 -1 -1 -1 -1 -1 -1 4 6
```

Sample Output-1:
----------
```
20
```

Explanation:
----------
Castle:
```
          1
        /   \
       4     3
      / \   / \
     2   4 2   5
              / \
             4   6
```

The best organized section starts at the room with a treasure value of 3, yielding a total treasure of 20.

Sample Input-2:
----------
```
4 3 -1 1 2
```

Sample Output-2:
----------
```
2
```

Explanation:
----------
Castle:
```
    4
   /
  3
 / \
1   2
```

The optimal organized section is just the single room with a treasure value of 2.

Sample Input-3:
----------
```
-4 -2 -5
```

Sample Output-3:
----------
```
0
```

Explanation: 
----------
Castle:
```
   -4
  /  \
-2   -5
```
 
Since all rooms contain negative treasure values, no beneficial organized section exists, so the maximum total treasure is 0.

Constraints:
----------
- The number of rooms in the castle ranges from 1 to 40,000.
- Each room’s treasure value is an integer between -40,000 and 40,000.

## Solution:

```java
import java.util.*;

class Node {
  int val;
  Node l = null, r = null;

  Node(int val) {
    this.val = val;
  }
}

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    int vals[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    System.out.println(maxOrganisedTreasure(vals));
    sc.close();
  }

  private static int maxOrganisedTreasure(int[] vals) {
    Node root = buildTree(vals);
    int[] max = { 0 };
    maxOrganised(root, max);
    return max[0];
  }

  private static Node buildTree(int[] vals) {
    if (vals.length == 0)
      return null;

    Node root = new Node(vals[0]);
    Queue<Node> q = new LinkedList<>();
    q.add(root);

    int i = 0;
    while (i < vals.length - 1) {
      Node curr = q.remove();

      if (vals[++i] != -1) {
        curr.l = new Node(vals[i]);
        q.add(curr.l);
      }

      if (i < vals.length - 1 && vals[++i] != -1) {
        curr.r = new Node(vals[i]);
        q.add(curr.r);
      }
    }

    return root;
  }

  private static int maxOrganised(Node root, int[] max) {
    if (root==null) return 0;

    int l=maxOrganised(root.l, max), r=maxOrganised(root.r, max);

    if ((l<=-1 || r<=-1) || 
        (l!=0 && root.val<=root.l.val) || 
        (r!=0 && root.r.val<=root.val))
      return -1;
    
    int curr = root.val+l+r;
    max[0] = Math.max(max[0], curr);
    return curr;
  }
}
```