#Coding/BSTree/Traversal 

Imagine you are an explorer who stumbles upon two ancient treasure chests. However, these aren’t ordinary chests—the coins inside are arranged in a mysterious, branching order. In each chest, the coins are hidden in secret compartments where lower valued coins are tucked away on one side and higher valued coins are placed on the opposite side. To unlock the legendary vault of riches, you must select one coin from the first chest and one from the second chest such that their magical values add up to a secret key number.
Your challange is to return true if you can unlock the legendary vault of riches, otherwise false.

Sample Input-1
----------
```
2 1 4
1 0 3
5
```

Sample Output-1:
----------
```
true
```

Explanation:
----------
Chest A:
```
    2
   / \
  1   4
```

Chest B:
```
    1
   / \
  0   3
```

Choosing the coin with value 2 from Chest A and the coin with value 3 from Chest B adds up to 5, unlocking the vault

Sample Input-2:
----------
```
0 -10 10
5 1 7 0 2
18
```

Sample Output-2:
----------
```
false
```

Explanation:
----------
Chest A:
```
    0
   / \
-10   10
```

Chest B:
```
      5
     / \
    1   7
   / \
  0   2
```

No combination of one coin from Chest A and one coin from Chest B 
sums to 18, so the vault remains sealed.

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

    int chests[][] = {
      Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray(),
      Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray()
    };

    int target = sc.nextInt();

    System.out.println(canUnlock(chests, target));

    sc.close();
  }

  private static boolean canUnlock(int[][] chests, int target) {
    Node roots[] = { buildBSTree(chests[0]), buildBSTree(chests[1]) };

    return twoSumBSTs(roots[0], roots[1], target);
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

  private static boolean twoSumBSTs(Node root1, Node root2, int target) {
    if (root1 == null || root2 == null) return false;

    if (root1.val+root2.val < target) return twoSumBSTs(root1.r, root2, target) || twoSumBSTs(root1, root2.r, target);
    else if (root1.val+root2.val > target) return twoSumBSTs(root1, root2.l, target) || twoSumBSTs(root1.l, root2, target);
    else return true;
  }
}
```