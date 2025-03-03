#Coding/LinkedLists/Traversal #Coding/TwoPointer 

Cliff Shaw is working on the singly linked list.
He is given a list of boxes arranged as singly linked list,
where each box is printed a positive number on it.

Your task is to help Mr Cliff to find the given list is equivalent to 
the reverse of it or not. If yes, print "true", otherwise print "false"

Input Format:
-------------
Line-1: space separated integers, boxes as list.

Output Format:
--------------
Print a boolean a value.


Sample Input-1:
---------------
```
3 6 2 6 3
```

Sample Output-1:
----------------
```
true
```


Sample Input-2:
---------------
```
3 6 2 3 6
```

Sample Output-2:
----------------
```
false
```

## Solution:

```java
import java.util.*;

class Node {
  int val;
  Node next=null;
  
  Node (int val) {
    this.val = val;
  }
}

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int[] vals = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    
    System.out.println(isPalindrome(vals));
    
    sc.close();
  }
  
  private static boolean isPalindrome(int[] vals) {
    Node head = buildList(vals);
    
    return checkPalindrome(head);
  }
  
  private static Node buildList(int[] vals) {
    Node curr=null, root=null;
    for (int val: vals) {
      if (root==null) curr = root = new Node(val);
      else {
        curr.next = new Node(val);
        curr = curr.next;
      }
    }
    return root;
  }
  
  private static boolean checkPalindrome(Node head) {
    Node s=head, f=head;
    
    while (f.next!=null && f.next.next!=null) {
      s = s.next;
      f = f.next.next;
    }
    
    s.next = reverse(s.next);
    
    while (s.next!=null) {
      if (head.val!=s.next.val) return false;
      head = head.next;
      s = s.next;
    }
    return true;
  }
  
  private static Node reverse(Node head) {
    Node prev=null, next, curr=head;
    
    while (curr!=null) {
      next = curr.next;
      curr.next = prev;
      prev = curr;
      curr = next;
    }
    
    return prev;
  }
}
```