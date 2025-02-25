#Coding/Trees/Traversal 

Imagine you're an archivist in the ancient Kingdom of Numeria, tasked with
Recording the royal lineage on a sacred scroll. Every monarch is identified
By a unique number, and the family tree unfolds in a very specific way.
Your mission is to transcribe this dynasty using a method that adheres
To the following customs:

Royal Inscription:
- Each monarch’s unique number is written as their signature on the scroll.

Enclosing Descendants:
- If a monarch has offspring, their descendants must be recorded inside parentheses immediately following the monarch’s number.
- The first-born (or primary heir) is enclosed in its own set of parentheses.
- If there is a second-born, their number is similarly enclosed, following the first-born’s parentheses.

Omitting Redundant Markings:
- Empty pairs of parentheses are generally left off the scroll to keep the Record neat.
- However, if a monarch has a second-born but first-born is no more, you must include an empty pair of parentheses to clearly mark the absence of a primary Heir—ensuring the lineage is accurately mapped.

Your task is to create such an inscription that perfectly reflects the
Hierarchical structure of the royal lineage.

Sample Input-1:
------
```
1 2 3 4
```

Sample Output-1:
------
```
1 (2 (4))(3)
```

Explanation:
------
```
    1
   / \
  2   3
 /
4
```

- Monarch 1 is the founding ruler. He has two heirs: Monarch 2 (first-born) and Monarch 3 (second-born).
- Monarch 2 has a single descendant, Monarch 4, recorded as his first-born.
- Since empty markings for non-existent descendants are omitted (because they don't add any information), the inscription is kept concise.
- Thus, the final transcription on the sacred scroll is: "1 (2 (4))(3)"

Sample Input-2:
------
```
1 2 3 -1 4
```

Sample Output-2:
------
```
1 (2 ()(4))(3)
```

Explanation:
```
  1
 / \
2   3
 \
  4
```

- Monarch 1 is the founding ruler with two heirs: Monarch 2 and Monarch 3.
- In this dynasty, Monarch 2 first-born is no more alive but does have a younger  descendant, Monarch 4.
- To ensure the record accurately reflects the gap in succession for Monarch 2, an empty pair of parentheses is added before representing Monarch 4. This explicitly marks the absence of a first-born heir.
- Therefore, the recorded inscription becomes: "1 (2 ()(4))(3)"

Sample Input-3:
------
```
1 2 3 4 -1 5 6 -1 7
```

Sample Output-3:
------
```
1 (2 (4 ()(7)))(3 (5)(6))
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

    System.out.print(familyTree(vals));

    sc.close();
  }

  private static String familyTree(int[] vals) {
    Node root = buildTree(vals);
    
    StringBuilder sb = new StringBuilder();
    familyString(root, sb);
    return sb.toString();
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

  private static void familyString(Node root, StringBuilder sb) {
    if (root==null) return;

    sb.append(root.val);

    if (root.l==null && root.r==null) return;
    
    sb.append("(");
    familyString(root.l, sb);
    sb.append(")");

    if (root.r == null) return;

    sb.append("(");
    familyString(root.r, sb);
    sb.append(")");
  }
}
```