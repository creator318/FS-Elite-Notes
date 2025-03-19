#Coding/Trie

Imagine you're a digital security analyst reviewing a suspicious email. The email’s content is a continuous string of characters, and you have a list of keywords commonly used in phishing scams. Your mission is to scan the email text and flag every segment that exactly matches one of these keywords. In other words, identify all index pairs \[i, j] such that the substring from position i to j in the email text is one of the suspicious keywords. Return these pairs sorted by their starting index, and if two pairs share the same starting index, sort them by their ending index.

Input Format
------------
Line-1: string STR(without any space)
Line-2: space separated strings, suspicious keywords[]

Output Format
-------------
Print the pairs\[i, j] in sorted order.

Sample Input-1:
----------
```
cybersecuritybreachalert
breach alert cyber
```

Sample Output-1: 
----------
```
0 4
13 18
19 23
```

Sample Input-2:
----------
```
phishphishingphish
phish phishing
```

Sample Output-2:
----------
```
0 4
5 9
5 12
13 17
```


Explanation:
----------
Notice that keywords can overlap - for instance, the word "phish" appears as part of the substring \[5,9] in addition to the complete "phishing" substring \[5,12].

Constraints:
----------
- 1 <= emailText.length <= 100  
- 1 <= suspiciousWords.length <= 20  
- 1 <= suspiciousWords\[i].length <= 50  
- emailText and each suspicious word consist of lowercase English letters.  
- All suspicious words are unique.

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
    
    String text = sc.nextLine().trim();
    String kws[] = sc.nextLine().split(" ");
    
    System.out.println(findKeywords(text, kws));
    
    sc.close();
  }
  
  private static List<List<Integer>> findKeywords(String text, String[] kws) {
    Node trie = new Node();
    
    for (String kw: kws) insert(trie, kw);
    
    return search(trie, text);
  }
  
  private static void insert(Node node, String word) {
    for (char ch: word.toCharArray()) {
      int idx = ch-'a';
      
      if (node.children[idx]==null) node.children[idx] = new Node();
      node = node.children[idx];
    }
    
    node.end = true;
  }
  
  private static List<List<Integer>> search(Node root, String word) {
    List<List<Integer>> res = new LinkedList<>();
    
    for (int i=0; i<word.length(); i++) {
      Node curr = root;
      for (int j=i; j<word.length(); j++) {
        int idx = word.charAt(j)-'a';
    
        if (curr.children[idx]==null) break;
        curr = curr.children[idx];
    
        if (curr.end) res.add(Arrays.asList(i, j));
      }
    }

    return res;
  }
}
```