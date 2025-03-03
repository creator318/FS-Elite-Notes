#Coding/TwoPointer 

Imagine you are a secret agent tasked with sending encoded messages. 
Each original message is a string of lowercase letters, and you can disguise 
it by replacing selected, non-adjacent segments with the count of characters 
in those segments. However, the encoding must follow strict rules: only 
non-empty segments can be replaced, replacements cannot be adjacent, and any 
numbers used must not have leading zeros.

For instance, the message "substitution" can be encoded in various ways, such as:

• "s10n" (keeping 's', replacing the next 10 characters with 10, and ending with 'n')  
• "sub4u4" (keeping "sub", replacing 4 characters, then 'u', and replacing 4 more characters)  
• "12" (replacing the entire message with its length)  
• "su3i1u2on" (using a different pattern of replacements)  
• "substitution" (leaving the message unaltered)

Invalid encodings include:

• "s55n" (because the replaced segments are adjacent)  
• "s010n" (the number 010 has a leading zero)  
• "s0ubstitution" (attempts to replace an empty segment)

Your task is: given an original message and an encoded version, 
determine if the encoded version is a valid secret code for the message.

Sample Input-1:
------
```
internationalization
i12iz4n
```
  
Sample Output-1: 
------
```
true  
```

Explanation:
------
"internationalization" can be encoded as "i12iz4n" by keeping 'i', 
replacing 12 letters, keeping "iz", replacing 4 letters, and then ending with 'n'.

Sample Input-2: 
------
```
apple
a2e
```
  
Sample Output-2: 
------
```
false
```  

Explanation: 
------
The encoding "a2e" does not correctly represent "apple".

Time Complexity:
------
O(n) where n=max(len(word),len(abbr))

Auxiliary Space:
------
O(1)

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    String word = sc.next(), abbr = sc.next();
    
    System.out.println(isValidAbbrevation(word, abbr));
    
    sc.close();
  }
  
  private static boolean isValidAbbrevation(String word, String abbr) {
    int w=0, a[]={ 0 };
    
    while (w<word.length() && a[0]<abbr.length()) {
      while (w<word.length() && a[0]<abbr.length() && ('0' > abbr.charAt(a[0]) || abbr.charAt(a[0]) > '9') && word.charAt(w)==abbr.charAt(a[0])) {
        w++; a[0]++;
      }
            
      if (w<word.length() && a[0]<abbr.length() && ('0'>abbr.charAt(a[0]) || abbr.charAt(a[0])>'9') && word.charAt(w)!=abbr.charAt(a[0])) return false;
      else if (w==word.length() && a[0]==abbr.length()) return true;

      int c = getVal(abbr, a);
      if (c<0 || (w+c)>word.length()) return false;
      w += c;
    }
    
    return w==word.length() && a[0]==abbr.length();
 }
  
  private static int getVal(String s, int[] i) {
    if (i[0]>=s.length() || s.charAt(i[0])=='-' || s.charAt(i[0])=='0') return -1; 
    
    int val = 0;

    while (i[0]<s.length() && '0'<=s.charAt(i[0]) && s.charAt(i[0])<='9')
      val = val*10 + s.charAt(i[0]++)-'0';
      
    return val;
  }
}
```
