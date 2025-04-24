#Coding/BackTracking 

In the ancient land of Palindoria, wizards write magical spells as strings of lowercase letters. However, for a spell to be effective, each segment of the spell must read the same forward and backward.

Given a magical spell 'spell', your task is to partition it into sequences of valid magical spell segments. 
Your goal is to help the wizard discover all valid combinations of magical spell segmentations.

Sample Input-1:
----------
```
aab
```
  
Sample Output-1:  
----------
```
[[a, a, b], [aa, b]]
```

Sample Input-2:  
----------
```
a 
```

Sample Output-2:  
----------
```
[[a]]
```

Spell Constraints:
----------
- The length of the spell is between 1 and 16 characters.  
- The spell contains only lowercase English letters. 

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    String s = sc.next();

    System.out.println(palindromeSubstrings(s));

    sc.close();
  }

  private static List<List<String>> palindromeSubstrings(String s) {
    List<List<String>> res = new LinkedList<>();

    backtrack(s, res, 0, new LinkedList<>());

    return res;
  }

  private static void backtrack(String s, List<List<String>> res, int pos, List<String> curr) {
    if (pos==s.length()) {
      res.add(new LinkedList<>(curr));
      return;
    }

    for (int i=pos; i<s.length(); i++) {
      String subs = s.substring(pos, i+1);

      if (new StringBuilder(subs).reverse().toString().equals(subs)) {
        curr.add(subs);
        backtrack(s, res, i+1, curr);
        curr.remove(curr.size()-1);
      }
    }
  }
}
```

