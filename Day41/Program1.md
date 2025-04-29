#Coding/DynamicProgramming 

You are participating in a futuristic space exploration mission where you must navigate through a series of planets aligned in a straight line. The planets are numbered from 0 to n-1, and you start your journey on planet 0.

You are given a 0-indexed array planets, where each element planets\[i] represents the maximum number of planets you can move forward from planet i. In simpler terms, standing on planet i, you can move to any planet from i+1 to i+planets\[i], as long as you don't exceed the last planet.

Your goal is to reach the final planet (planet n-1) in the fewest number of moves possible.

It is guaranteed that a path to the final planet always exists.

Return the minimum number of moves needed to reach planet n-1.

Sample Input-1:
----------
```
2 3 1 0 4
```

Sample Output-1:
----------
```
2
```

Explanation:
----------
- Move from planet 0 to planet 1.
- Move from planet 1 to planet 4 (last planet).

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String args[]) {
    Scanner sc = new Scanner(System.in);

    int steps[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();

    System.out.println(minMoves(steps));
    sc.close();
  }

  private static int minMoves(int[] steps) {
    int dp[] = new int[steps.length];
    
    return getMoves(steps, 0, dp);
  }

  private static int getMoves(int[] steps, int pos, int[] dp) {
    if (pos>=steps.length-1) return 0;

    int min = Integer.MAX_VALUE;
    for (int i=pos+1; i<=Math.min(pos+steps[pos], steps.length-1); i++) {
      min = Math.min(min, getMoves(steps, i, dp));
    }

    return min==Integer.MAX_VALUE ? min : min+1;
  }
}
```