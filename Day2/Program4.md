#Coding/Trees/Construction #Coding/Trees/Traversal #Coding/BFS 

You are developing an application for a garden management system where each tree 
In the garden is represented as a binary tree structure. The system needs to 
Allow users to plant new trees in a systematic way, ensuring that each tree is 
Filled level by level.

A gardener wants to:
 - Plant trees based on user input.
 - Ensure trees grow in a balanced way by filling nodes level by level.
 - Inspect the garden layout by performing an in-order traversal, which helps analyze the natural arrangement of trees.

Your task is to implement a program that:
    - Accepts a list of N tree species (as integers).
    - Builds a binary tree using level-order insertion.
    - Displays the in-order traversal of the tree.

Input Format:
-------------
- An integer N representing the number of tree plants.
- A space-separated list of N integers representing tree species.

Output Format:
--------------
A list of integers, in-order traversal of tree.


Sample Input:
-------------
```
7
1 2 3 4 5 6 7
```

Sample Output:
--------------
```
4 2 5 1 6 3 7
```


Explanation:
------------
The tree looks like this:
```
        1
       / \
      2   3
     / \ / \
    4  5 6  7
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

class Tree {
    static Node root = null;
    
    Tree(int[] vals) {
        if (vals.length == 0) return;
        
        root = new Node(vals[0]);
        Queue<Node> q = new LinkedList<>();
        q.add(root);
        
        int i = 0;
        while (i<vals.length-1) {
            Node curr = q.remove();
            
            curr.l = new Node(vals[++i]);
            q.add(curr.l);
            
            if (i<vals.length-1) {
                curr.r = new Node(vals[++i]);
                q.add(curr.r);
            }
        }
    }
        
    public List<Integer> inOrder() {
        List<Integer> res = new LinkedList<>();
        inOrder(root, res);
        return res;
    }
    
    private void inOrder(Node root, List<Integer> res) {
        if (root==null) return;
        
        inOrder(root.l, res);
        res.add(root.val);
        inOrder(root.r, res);
    } 
}

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[] vals = new int[n];
        for (int i=0; i<n; i++) vals[i] = sc.nextInt();

        Tree t = new Tree(vals);
        List<Integer> inOrder = t.inOrder();
        
        for (int i: inOrder) System.out.print(i + " ");
        System.out.println();

        sc.close();
    }
}
```
