#Coding/Backtracking 

Given two strings S1 and S2, find if S2 can match S1 or not.

A match that is both one-to-one (an injection) and onto (a surjection), i.e. a function which relates each letter in string S1 to a separate and distinct non-empty substring in S2, where each non-empty substring in S2 also has a corresponding letter in S1.

Return true,if S2 can match S1. Otherwise false.

Input Format:
-------------
Line-1 -> Two strings S1 and S2

Output Format:
--------------
Print a boolean value as result.

Sample Input-1:
---------------
```
abab kmitngitkmitngit
```

Sample Output-1:
----------------
```
true
```

Sample Input-2:
---------------
```
aaaa kmjckmjckmjckmjc
```

Sample Output-2:
----------------
```
true
```

Sample Input-3:
---------------
```
mmnn pqrxyzpqrxyz
```

Sample Output-3:
----------------
```
false
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    String s1 = sc.next(), s2 = sc.next();

    System.out.println(isMatching(s1, s2));

    sc.close();
  }

  private static boolean isMatching(String s1, String s2) {
    Map<Character, String> map = new HashMap<>();

    int curr[] = {0, 0};
    return backtrack(s1, s2, map, curr);
  }

  private static boolean backtrack(String s1, String s2, Map<Character, String> map, int[] curr) {
    if (curr[0]==s1.length() && curr[1]==s2.length()) return true;
    else if (curr[0]==s1.length() || curr[1]==s2.length()) return false;
    
    if (map.containsKey(s1.charAt(curr[0]))) {
      if (!s2.substring(curr[1]).startsWith(map.get(s1.charAt(curr[0])))) return false;
      
      return backtrack(s1, s2, map, new int[] {curr[0]+1, curr[1] + map.get(s1.charAt(curr[0])).length()});
    }

    for (int i=curr[1]; i<s2.length(); i++) {
      String sub = s2.substring(curr[1], i+1);
      
      if (!map.containsValue(sub)) {
        map.put(s1.charAt(curr[0]), sub);
        
        if (backtrack(s1, s2, map, new int[] {curr[0]+1, i+1})) return true;

        map.remove(s1.charAt(curr[0]));
      }
    }

    return false;
  }
}
```