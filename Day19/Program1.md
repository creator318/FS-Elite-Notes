#Coding/Strings #Coding/BitManipulation 

AlphaCipher is a string formed from another string by rearranging its letters

You are given a string S.
Your task is to check, can any one of the AlphaCipher is a palindrome or not.

Input Format:
-------------
A string S

Output Format:
--------------
Print a boolean value.


Sample Input-1:
---------------
```
carrace
```

Sample Output-1:
----------------
```
true
```


Sample Input-2:
---------------
```
code
```

Sample Output-2:
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

    String s = sc.next();
    
    System.out.println(isPalindrome(s));
    
    sc.close();
  }

  private static boolean isPalindrome(String s) {
    int mask = 0;
    for (char ch: s.toCharArray()) mask ^= 1 << (ch-'a');
    return (mask & (mask-1)) == 0;
  }
}
```