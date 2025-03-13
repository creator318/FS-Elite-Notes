#Coding/Trees/Construction #Coding/Trees/Traversal #Coding/BFS 

Imagine you’re decoding a secret message that outlines the hierarchical structure 
Of a covert spy network. The message is a string composed of numbers and parentheses. 
Here’s how the code works:

- The string always starts with an agent’s identification number, this is the 
  Leader of the network.
- After the leader’s ID, there can be zero, one, or two segments enclosed in 
  Parentheses. Each segment represents the complete structure of one subordinate 
  Network.
- If two subordinate networks are present, the one enclosed in the first (leftmost) 
  Pair of parentheses represents the left branch, and the second (rightmost) 
  Represents the right branch.

Your mission is to reconstruct the entire spy network hierarchy based on this 
Coded message.

Sample Input-1:
------
```
4(2(3)(1))(6(5))
```

Sample Output-1:
------
```
[4, 2, 6, 3, 1, 5]
```

Explanation:
------
```
    4
   / \
  2   6
 / \ /
3  1 5

```
Agent 4 is the leader.
Agent 2 (with its own subordinates 3 and 1) is the left branch.
Agent 6 (with subordinate 5) is the right branch.

Sample Input-2:
------
```
4(2(3)(1))(6(5)(7))
```

Sample Output-2:
------
```
[4, 2, 6, 3, 1, 5, 7]
```


Explanation:
------
```
    4
   / \
  2   6
 / \ / \
3  1 5  7

```
Agent 4 leads the network.
Agent 2 with subordinates 3 and 1 forms the left branch.
Agent 6 with subordinates 5 and 7 forms the right branch.

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
    
    String s = sc.nextLine();
    
    System.out.println(treeLevelOrder(s));
    
    sc.close();
  }
  
  private static List<Integer> treeLevelOrder(String s) {
    Node root = buildTree(s, 0, s.length()-1);
    
    return levelOrder(root);
  }
  
  private static Node buildTree(String s, int l, int r) {
    int[] i = { l };

    Node root = new Node(getVal(s, i, r));

    int[] lp = nextParams(s, i, r);
    if (lp[0] == -1 && lp[1] == -1)
      return root;
    root.l = buildTree(s, lp[0], lp[1]);

    int[] rp = nextParams(s, i, r);
    if (rp[0] == -1 && rp[1] == -1)
      return root;
    root.r = buildTree(s, rp[0], rp[1]);

    return root;
  }

  private static int getVal(String s, int[] i, int r) {
    int val = 0;
    boolean negative = false;
    if (s.charAt(i[0]) == '-') {
      i[0]++;
      negative = true;
    }
    while (i[0] <= r && '0' <= s.charAt(i[0]) && s.charAt(i[0]) <= '9')
      val = val*10 + (s.charAt(i[0]++)-'0')*(negative ? -1 : 1);
      
    return val;
  }
    
  private static int[] nextParams(String s, int[] i, int r) {
    int[] res = {-1, -1};
    
    while (i[0]<r && s.charAt(i[0])!='(') i[0]++;
    
    if (i[0]>=r) return res;

    res[0] = ++i[0];
    int paren = 1;
    while (i[0]<r) {
      if (s.charAt(i[0])=='(') paren++;
      else if (s.charAt(i[0])==')') paren--;
        
      if (paren==0) break;
      i[0]++;
    }
    res[1] = --i[0];
    
    return res;
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