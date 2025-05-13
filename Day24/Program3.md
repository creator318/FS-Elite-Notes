#Coding/Backtracking 

Imagine you're a top-secret agent receiving an encrypted directive from headquarters. The message comes as a string of digits, and each digit (from 2 to 9) is a cipher for a set of potential code letters. To uncover the true instruction, you must translate the string into every possible combination of letters by substituting each digit with its corresponding set of letters. The final decoded messages listed in lexicographycal order.

Below is the mapping of digits to letters (as found on a traditional telephone keypad):

| Digit | Letters    |
| ----- | ---------- |
| 2     | a, b, c    |
| 3     | d, e, f    |
| 4     | g, h, i    |
| 5     | j, k, l    |
| 6     | m, n, o    |
| 7     | p, q, r, s |
| 8     | t, u, v    |
| 9     | w, x, y, z |

Note: The digit 1 does not correspond to any letters.

Sample Input-1:
----------
```
23
```

Sample Output-1: 
----------
```
[ad, ae, af, bd, be, bf, cd, ce, cf]
```

Sample Input-2:
----------
```
2 
```

Sample Output-2:
----------
```
[a, b, c]
```


Constraints:
----------
- 0 <= digits. Length <= 4  
- Each digit in the input is between '2' and '9'.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    String cipher = sc.next();
    
    System.out.println(findCodes(cipher));
    
    sc.close();
  }
  
  private static List<String> findCodes(String cipher) {
    final Map<Character, String> map = new HashMap<>() {{
      put('2', "abc");
      put('3', "def");
      put('4', "ghi");
      put('5', "jkl");
      put('6', "mno");
      put('7', "pqrs");
      put('8', "tuv");
      put('9', "wxyz");
    }};
    
    List<String> res = new LinkedList<>();
    
    if (cipher.length()==0) return res;
    
    backtrack(cipher, "", 0, res, map);
    Collections.sort(res);
    return res;
  }
  
  private static void backtrack(String cipher, String code, int pos, List<String> res, Map<Character, String> map) {
    if (pos==cipher.length()) {
      res.add(code);
      return;
    }
    
    String curr = map.get(cipher.charAt(pos));
    
    for (int i=0; i<curr.length(); i++) backtrack(cipher, code+curr.charAt(i), pos+1, res, map);
  }
}
```