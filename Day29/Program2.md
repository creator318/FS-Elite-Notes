#Coding/Trie 

At university of Chicago a Computer Science programing faculty as a part of teaching passion, in order to make newly joined students more enthusiastic in learning the subject he is given a problem at the first day of semester. The student who solved it first, will be awarded with a cash prize. In regard to this he asked the students to work on concept related to strings, he gave a task to read a word and find the count of all the turn of phrases of the word, and the phrases should be distinct.

Now it’s your time to create a method which satisfies the above program.
The turn of phrases of a word is obtained by deleting any number of characters (possibly zero) from the front of the word and any number of characters (possibly zero) from the back of the word.

Input Format:
-------------
A single string, the word contains only lowercase alphabets \[a-z].

Output Format:
--------------
Print an integer, number of distinct phrases possible.


Sample Input-1:
---------------
```
aabbaba
```

Sample Output-1:
----------------
```
21
```

Explanation:
-------------
The turn of phrases of the word, which are distinct as follows:
a, b, aa, bb, ab, ba, aab, abb, bab, bba, aba, aabb, abba, bbab, baba, aabba, abbab, bbaba, aabbab, abbaba, aabbaba


Sample Input-2:
---------------
```
kmithyd
```

Sample Output-2:
----------------
```
28
```

## Solution

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
    
    String word = sc.next();

    System.out.println(uniquePhrases(word));
    
    sc.close();
  }
  
  private static int uniquePhrases(String word) {
    Node trie = new Node();
    int count = 0;

    for (int i=0; i<word.length(); i++) 
      count += insert(trie, word.substring(i));

    return count;
  }
  
  private static int insert(Node node, String word) {
    int count = 0;

    for (char ch: word.toCharArray()) {
      int idx = ch-'a';
  
      if (node.children[idx]==null) {
        node.children[idx] = new Node();
        count++;
      }
      node = node.children[idx];
    }
    
    node.end = true;
    return count;
  }
}
```