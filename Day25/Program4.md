#Coding/Trees

Imagine you are the curator of a historic library, where books are arranged in a unique catalog system based on their publication years. The library’s archive is structured like a hierarchical tree, with each book’s publication year stored at a node. You are given the nodes of this catalog tree starting with main node and a list of query years.

For each query year, you need to find two publication years:
- The first is the latest year in the archive that is less than or equal to the query year. If no such book exists, use -1.
- The second is the earliest year in the archive that is greater than or equal to the query year. If no such book exists, use -1.

Display the results as an list of pairs, where each pair corresponds to a query.

Sample Input-1:
----------
```
2006 2002 2013 2001 2004 2009 2015 2014
2002 2005 2016
```

Sample Output-1:
----------
```
[[2002, 2002], [2004, 2006], [2015, -1]] 
```

Explanation
----------
Archive Structure:
```
          2006
         /    \
     2002      2013
     /   \    /   \
  2001 2004  2009 2015
                  /
                2014
```
- For the query 2002, the latest publication year that is <= 2002 is 2002, and the earliest publication year that is >= 2002 is also 2002.  
- For the query 2005, the latest publication year that is <= 2005 is 2004, and the earliest publication year that is >= 2005 is 2006.  
- For the query 2016, the latest publication year that is <= 2016 is 2015, but there is no publication year >= 2016, so we output -1 for the second value.

Sample Input-2:
----------
```
2004 2009
2003
```

Sample Output-2:
----------
```
[[-1, 2004]]
```

Explanation:
----------
- For the query 2003, there is no publication year <= 2003, while the earliest publication year that is >= 2003 is 2004.

Constraints:
----------
- The total number of books in the archive is in the range \[2, $10^5$].
- Each publication year is between 1 and $10^6$.
- The number of queries n is in the range \[1, $10^5$].
- Each query year is between 1 and $10^6$.

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
    int queries[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    
    System.out.println(findBounds(vals, queries));
    
    sc.close();
  }
  
  private static List<List<Integer>> findBounds(int[] vals, int[] queries) {
    Node root = buildBSTree(vals);

    List<List<Integer>> res = new LinkedList<>();
    
    for (int q: queries) {
      int curr[] = {-1, -1};
      findLower(root, q, curr);
      findUpper(root, q, curr);
      res.add(Arrays.asList(curr[0], curr[1]));
    }

    return res;
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

  private static void findLower(Node root, int query, int[] curr) {
    if (root==null) return;
    
    if (root.val>query) findLower(root.l, query, curr);
    else if (root.val==query) curr[0] = root.val;
    else {
      curr[0] = root.val;
      findLower(root.r, query, curr);
    }
  }

  private static void findUpper(Node root, int query, int[] curr) {
    if (root==null) return;

    if (root.val<query) findUpper(root.r, query, curr);
    else if (root.val==query) curr[1] = root.val;
    else {
      curr[1] = root.val;
      findUpper(root.l, query, curr);
    }
  }
}
```