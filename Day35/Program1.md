#Coding/BackTracking 

Captain Raynor is on a mission to decode an alien transmission. The transmission contains a single long string S, and it is believed to be made up of multiple unique signal chunks, sent one after another with no spaces.

Your task is to help Captain Raynor split the transmission into the maximum number of non-empty, unique signal chunks such that when all chunks are concatenated in order, they exactly recreate the original transmission S.

Your goal is to maximize the number of unique chunks the message can be split into.

Input Format:
---------------
Line-1: A string S representing the alien transmission.

Output Format:
---------------
Print a single integer – the maximum number of distinct chunks the string can be split into.

Sample Input-1:
---------------
```
banana
```

Sample Output-1:
----------------
```
4
```

Explanation: 
------------
One valid way to split the string is: "b", "a", "n", "ana".
This keeps all chunks unique.
Another way like "b", "a", "n", "an", "a" is invalid because "a" appears twice.

Sample Input-2:
---------------
```
mississippi
```

Sample Output-2:
----------------
```
7
```

Explanation: 
------------
One valid way to split it is: "m", "i", "s", "si", "ssi", "p", "pi".
All chunks are distinct and together recreate the original transmission.

NOTE: Only contiguous chunks (i.e., substrings) are allowed. Subsequence-based splitting is not permitted.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    String s = sc.next();

    System.out.println(maxChunks(s));

    sc.close();
  }

  private static int maxChunks(String s) {
    int res[] = {0};

    Set<String> chunks = new HashSet<>();
    backtrack(s, chunks, 0, res);

    return res[0];
  }

  private static void backtrack(String s, Set<String> chunks, int curr, int[] max) {
    if (curr==s.length()) {
      max[0] = Math.max(max[0], chunks.size());
      return;
    }

    for (int i=curr; i<s.length(); i++) {
      String sub = s.substring(curr, i+1);

      if (!chunks.contains(sub)) {
        chunks.add(sub);
        backtrack(s, chunks, i+1, max);
        chunks.remove(sub);
      }
    }
  }
}
```