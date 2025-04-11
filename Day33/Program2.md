
There are n football players standing in the ground, coach wants to know the P-th largest height of the players. Given an array of heights\[] and the value of P. Help the coach to find the P'th largest height.

Note: You are supposed to print the P'th largest height in the sorted order of heights\[]. Not the P'th distinct height.

Input Format:
-------------
Line-1: Size of array n and P value(space separated)
Line-2: Array elements of size n.

Output Format:
--------------
Print P'th largest height.

Sample input-1:
---------------
```
8 2
1 2 1 3 4 5 5 5
```

Sample output-1:
----------------
```
5
```

Sample input-2:
---------------
```
6 3
2 4 3 1 2 5
```

Sample output-2:
----------------
```
3
```

## Solution

```java
import java.util.*;

class Node {
  int val, priority, size;
  Node left, right;

  Node(int val) {
    this.val = val;
    priority = new Random().nextInt();
    size = 1;
  }
}

class Treap {
  private Node root;

  public void insert(int v) {
    root = insert(root, v);
  }

  private Node insert(Node n, int v) {
    if (n == null) return new Node(v);
    if (v <= n.val) {
      n.left = insert(n.left, v);
      if (n.left.priority > n.priority) return rotateRight(n);
    } else {
      n.right = insert(n.right, v);
      if (n.right.priority > n.priority) return rotateLeft(n);
    }
    update(n);
    return n;
  }

  private Node rotateLeft(Node x) {
    Node y = x.right;
    x.right = y.left;
    y.left = x;
    
    update(x);
    update(y);
    
    return y;
  }

  private Node rotateRight(Node y) {
    Node x = y.left;
    y.left = x.right;
    x.right = y;
    
    update(y);
    update(x);
    
    return x;
  }

  private void update(Node n) {
    n.size = 1 + size(n.left) + size(n.right);
  }

  private int size(Node n) {
    return n == null ? 0 : n.size;
  }
  
  public int kthLargest(int k) {
    return kth(root, size(root) - k + 1);
  }

  private int kth(Node n, int k) {
    int ls = size(n.left);
    if (k == ls + 1) return n.val;
    if (k <= ls) return kth(n.left, k);
    return kth(n.right, k - ls - 1);
  }
}

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt(), p = sc.nextInt();
    
    int[] a = new int[n];
    for (int i = 0; i < n; i++) a[i] = sc.nextInt();
    
    System.out.println(kthLargesr(a, p));
    
    sc.close();
  }

  private static int kthLargesr(int[] a, int p) {
    Treap t = new Treap();
    for (int h: a) t.insert(h);
    
    return t.kthLargest(p);
  }
}
```