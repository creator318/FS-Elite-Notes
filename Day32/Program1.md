#Coding/SlidingWindow 

A detective is investigating a case involving a secret message hidden within a longer note. The detective knows that the culprit rearranged the letters of a short code-word into multiple secret locations within a larger note.

Given two strings, note (the longer text) and codeWord (the short secret code), your task is to help the detective find all starting positions within the note where any rearrangement or shuffled of codeWord is located.

Input Format:
-------------
Single line space separated strings, two words.

Output Format:
--------------
Print the list of integers, indices.


Sample Input-1:
---------------
```
bacdgabcda abcd
```
 
Sample Output-1:
----------------
```
[0, 5, 6]
```

Explanation:
- At index 0: "bacd" is an anagram of "abcd"
- At index 5: "abcd" matches exactly
- At index 6: "bcda" is an anagram of "abcd"
Thus, the positions \[0, 5, 6] are returned.

Sample Input-2:
---------------
```
bacacbacdcab cab
```

Sample Output-2:
----------------
```
[0, 3, 4, 5, 9]
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    String txt = sc.next(), target = sc.next();
    
    System.out.println(findAnagrams(txt, target));
    
    sc.close();
  }
  
  private static List<Integer> findAnagrams(String txt, String target) {
    int n = txt.length(), k = target.length();

    Map<Character, Integer> targetMap = new HashMap<>(), map = new HashMap<>();
    for (int i=0; i<k; i++) {
      targetMap.put(target.charAt(i), targetMap.getOrDefault(target.charAt(i), 0)+1);
      map.put(txt.charAt(i), map.getOrDefault(txt.charAt(i), 0)+1);
    }
    
    List<Integer> res = new LinkedList<>();
    for (int i=0; i<=n-k; i++) {
      boolean flag=true;
      for (Map.Entry<Character, Integer> e: targetMap.entrySet()) {
        if (map.get(e.getKey())!=e.getValue()) {
          flag = false;
          break;
        }
      }
      if (flag) res.add(i);
      
      if (i==n-k) continue;
      map.put(txt.charAt(i), map.get(txt.charAt(i))-1);
      map.put(txt.charAt(i+k), map.getOrDefault(txt.charAt(i+k), 0)+1);
    }

    return res;
  }
}
```