#Coding/Trie 

Imagine you're playing a fantasy video game where secret level codes unlock new worlds. These codes are strings made up of letters, and a level code is only considered valid if every shorter code formed by its leading characters has been discovered along the journey. In other words, a code is unlockable only when all of its prefixes are also present in your collection.

Given a list of strings representing the level codes you’ve collected, find the longest valid level code such that every prefix of that code is in the list. If there is more than one valid code of the same length, choose the one that comes first in alphabetical order. If no such code exists, return an empty string.

Input Format
-------------
Line1: Space separated codes (strings)
 
Output Format
--------------
string 


Sample Input-1:
----------
```
m ma mag magi magic magici magicia magician magicw
```

Sample Output-1:
----------
```
magician
```

Explanation:
----------
The code "magician" is unlockable because every prefix - "m", "ma", "mag", "magi", "magic", "magici", and "magicia" - is present in the list. Although "magicw" appears too, it fails since its prefix "magica" is missing.


Sample Input-2:
----------
```
a banana app appl ap apply apple
```

Sample Output-2: 
----------
```
apple
```

Explanation:
----------
Both "apple" and "apply" have every prefix in the list, but "apple" comes first in alphabetical order.

Sample Input-3:
----------
```
abc bc ab abcd
```

Sample Output-3: 
----------
```

```

Explanation:
----------
No code can be unlocked as all have different prefixes.

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
    
    System.out.println(longestValid(words));
    
    sc.close();
  }
  
  private static String longestValid(String[] words) {
    Node trie = new Node();
    String res = "";

    Arrays.sort(words);

    for (String word: words) {
      insert(trie, word);

      if (search(trie, word) && word.length()>res.length()) res = word;
    }
    
    return res;
  }
  
  private static void insert(Node node, String word) {
    for (char ch: word.toCharArray()) {
      int idx = ch-'a';
      
      if (node.children[idx]==null) node.children[idx] = new Node();
      node = node.children[idx];
    }
    
    node.end = true;
  }
  
  private static boolean search(Node node, String word) {    
    for (char ch: word.toCharArray()) {
      int idx = ch-'a';

      if (!node.children[idx].end) return false;
      node = node.children[idx];
    }

    return true;
  }
}
```
