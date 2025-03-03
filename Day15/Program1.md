#Coding/Strings

Imagine you are an artisan tasked with assembling a decorative mosaic from a 
Collection of uniquely colored tiles. Each tile is marked with a character, 
And your challenge is to rearrange these tiles to create a design that mirrors 
Itself perfectly from left to right. 
Your goal is to determine whether the available tiles can be arranged to form 
Such a symmetric pattern. Print true if a symmetric design is possible,
And false otherwise.

Input format:
------
A string representing the characters on the tiles.

Output format:
------
Print a boolean value

Sample Input-1:
-----
```
work
```

Sample Output-1:
------
```
false
```

Sample Input-2: 
------
```
ivicc
```

Sample Output-2:
------
```
true
```

Constraints:
------
1 <= string. Length <= 5000
Tile characters are all lowercase English letters.

## Solution :

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    String s = sc.next();
    
    System.out.println(transformablrPalindrome(s));
    
    sc.close();
  }
  
  private static boolean transformablrPalindrome(String s) {
    Map<Character, Integer> map = new HashMap<>();
    
    for (int i=0; i<s.length(); i++) map.put(s.charAt(i), map.getOrDefault(s.charAt(i), 0)+1);
    
    int odd = 0;
    
    for (int i: map.values()) if (i%2==1) odd++;

    return odd == (s.length()%2==1 ? 1 : 0);
  }
}
```