#Coding/DisjointSetUnion

In the world of secret codes and cryptography, you are entrusted with deciphering a hidden message. 
You have two encoded keys, key 1 and key 2, both of equal length. Each character 
In key 1 is paired with the corresponding character in key 2. 

This relationship follows the standard rules of an equivalence cipher:
- Self-Mapping: Every character inherently maps to itself.  
- Mutual Mapping: If a character from key 1 maps to one in key 2, then that character in key 2 maps back to the one in key 1.  
- Chain Mapping: If character A maps to B, and B maps to C, then A, B, and C are all interchangeable in this cipher.

Using this mapping, you must decode a cipherText, by replacing every character with the smallest equivalent character from its equivalence group. 
The result should be the relatively smallest possible decoded message.


Input Format:
-------------
Three space separated strings, key 1 , key 2 and cipherText

Output Format:
--------------
Print a string, decoded message which is relatively smallest string of cipherText.

Sample Input-1:
------
```
attitude progress apriori
```

Sample Output-1:
------
```
aaogoog
```


Explanation:
------
The mapping pairs form groups: \[a, p], \[o, r, t], \[g, i], \[e, u], 
\[d, e, s]. By substituting each character in cipherText with the smallest from 
Its group, you decode the message to "aaogoog".

Constraints:  
------
 - 1 <= key 1. Length, key 2. Length, cipherText. Length <= 1000  
 - key 1. Length == key 2. Length  
 - key 1, key 2, and cipherText consist solely of lowercase English letters.


## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    String k1 = sc.next(), k2 = sc.next();
    String cipher = sc.next();
    
    System.out.println(decipher(k1, k2, cipher));
    
    sc.close();
  }
  
  private static String decipher(String k1, String k2, String cipher) {
    Map<Character, Character> parent = new HashMap<>();
    for (char ch='a'; ch<='z'; ch++) parent.put(ch, ch);
    
    for (int i=0; i<k1.length(); i++) union(k1.charAt(i), k2.charAt(i), parent);
    
    StringBuilder sb = new StringBuilder();
    for (char ch: cipher.toCharArray()) sb.append(find(ch, parent));
    
    return sb.toString();
  }
  
  private static char find(char ch, Map<Character, Character> parent) {
    if (parent.get(ch)!=ch) parent.put(ch, find(parent.get(ch), parent));
    return parent.get(ch);
  }
  
  private static void union(char ch1, char ch2, Map<Character, Character> parent) {
    char rootCh1=find(ch1, parent), rootCh2=find(ch2, parent);
    
    if (rootCh1==rootCh2) return;
    
    if (rootCh1<rootCh2) parent.put(rootCh2, rootCh1);
    else parent.put(rootCh1, rootCh2);
  }
}
```