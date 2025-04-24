#Coding/BackTracking 

You are given a crystal with an energy level n. Your goal is to discover all the different ways this crystal could have been created by combining smaller shards.

Each combination must:
- Use only shards with energy values between 2 and n - 1.
- Be represented as a list of shard values whose product equals n.
- Use any number of shards (minimum 2), and the order doesn't matter.

Your task is to return all unique shard combinations that can multiply together to recreate the original crystal.

Sample Input-1:
---------
```
28
```

Sample Output-1:
---------
```
[[2, 14], [2, 2, 7], [4, 7]]
```

Sample Input-2:
----------
```
23
```

Sample Output-2:
---------
```
[]
```

Constraints:
---------
- 1 <= n <= $10^4$
- Only shards with energy between 2 and n - 1 can be used.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt();

    System.out.println(validCombinations(n));

    sc.close();
  }

  private static List<List<Integer>> validCombinations(int n) {
    List<List<Integer>> res = new LinkedList<>();

    backtrack(n, 2, new LinkedList<>(), res);

    return res;
  }

  private static void backtrack(int n, int start, List<Integer> curr, List<List<Integer>> res) {
    if (n<2) {
      if (curr.size()>=2) res.add(new LinkedList<>(curr));
      return;
    }
    
    for (int i=start; i<=n; i++) {
      if (n%i==0) {
        curr.add(i);
        backtrack(n/i, i, curr, res);
        curr.remove(curr.size()-1);
      }
    }
  }
}
```