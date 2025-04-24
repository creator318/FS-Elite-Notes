#Coding/Trie 

Mr Suresh is working with the plain text P, a list of words w\[], He is converting P into C \[the cipher text], C is valid cipher of P, if the following rules are followed:
- The cipher-text C is a string ends with '$' character.
- Every word, w[i] in w[], should be a substring of C, and the substring should have $ at the end of it.

Your task is to help Mr Suresh to find the shortest Cipher C, and return its length.

Input Format:
-------------
Single line of space separated words, w\[].

Output Format:
--------------
Print an integer result, the length of the shortest cipher.


Sample Input-1:
---------------
```
kmit it ngit
```

Sample Output-1:
----------------
```
10
```

Explanation:
------------
A valid cipher C is "kmit\$ngit\$".
w\[0] = "kmit", the substring of C, and the '$' is the end character after "kmit"
w\[1] = "it", the substring of C, and the '$' is the end character after "it"
w\[2] = "ngit", the substring of C, and the '$' is the end character after "ngit"


Sample Input-2:
---------------
```
ace
```

Sample Output-2:
----------------
```
4
```

Explanation:
------------
A valid cipher C is "ace\$".
w\[0] = "ace", the substring of C, and the '\$' is the end character after "ace"

## Solution:

```java
import java.util.*;

class Node {
  Node[] children;
  boolean end=false;
  
  Node() {
    children = new Node[26];
  }
}

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner (System.in);
    
    String words[] = sc.nextLine().split(" ");
    
    System.out.println(shortestCipherLength(words));
    
    sc.close();
  }
  
  private static int shortestCipherLength(String[] words) {
    Node trie = new Node();
    
    for (String w: words) 
      insert(trie, new StringBuilder(w).reverse().toString());
      
    return cipherLength(trie, 0);
  }

  
  private static void insert(Node node, String word) {
    for (char ch: word.toCharArray()) {
      int idx = ch-'a';
      
      if (node.children[idx]==null) node.children[idx] = new Node();
      node = node.children[idx];
    }
    
    node.end = true;
  }
  
  private static int cipherLength(Node node, int depth) {
    int sum = 0;
    boolean isLeaf = true;
    
    for (int i=0; i<26; i++)
      if (node.children[i] != null) {
        isLeaf = false;
        sum += cipherLength(node.children[i], depth+1);
      }
    
    if (isLeaf && depth>0) sum += depth+1;
    
    return sum;
  }
}
```