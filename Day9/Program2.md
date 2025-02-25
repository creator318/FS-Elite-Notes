#Coding/Trees/Splitting 

Balbir Singh is working with Binary Trees.
The elements of the tree is given in the level order format.
Balbir has a task to split the tree into two parts by removing only one edge
In the tree, such that the product of sums of both the splitted-trees should be maximum.

You will be given the root of the binary tree.
Your task is to help the Balbir Singh to split the binary tree as specified.
Print the product value, as the product may be large, print the (product % 1000000007)

NOTE: Please do consider the node with data as '-1' as null in the given trees.

Input Format:
-------------
Space separated integers, elements of the tree.

Output Format:
--------------
Print an integer value.

Sample Input-1:
---------------
```
1 2 4 3 5 6
```

Sample Output-1:
----------------
```
110
```

Explanation:
------------
If you split the tree by removing edge between 1 and 4,
Then the sums of two trees are 11 and 10. So, the max product is 110.

Sample Input-2:
---------------
```
3 2 4 3 2 -1 6
```

Sample Output-2:
----------------
```
100
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

    System.out.println(maxProductSplit(vals));

    sc.close();
  }

  private static int maxProductSplit(int[] vals) {
    Node root = buildTree(vals);

    return splitProduct(root);
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
  
  private static int sum(Node root, Set<Integer> sums) {
    if (root==null) return 0;

    int lsum=sum(root.l, sums), rsum=sum(root.r, sums);
    sums.add(lsum);
    sums.add(rsum);

    return root.val + lsum + rsum;
  }

  private static int splitProduct(Node root) {
    int max = Integer.MIN_VALUE;
    
    Set<Integer> sums = new HashSet<>();
    int total = sum(root, sums);

    for (int i: sums) max = Math.max(max, (total-i)*i);
    
    return max % 1000000007;
  }
}
```