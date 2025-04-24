#Coding/DynamicProgramming

You are a stealthy archaeologist exploring a circular ring of ancient tombs located deep within a jungle. Each tomb holds a certain number of precious artifacts. 
However, these tombs are protected by an ancient magical curse: if you disturb two adjacent tombs during the same night, the entire ring activates a trap that seals you in forever.

The tombs are arranged in a perfect circle, meaning the first tomb is adjacent to the last. You must plan your artifact retrieval carefully to maximize the number of artifacts collected in a single night without triggering the curse.

Given an integer array artifacts representing the number of artifacts in each tomb, return the maximum number of artifacts you can collect without disturbing any two adjacent tombs on the same night.

Input Format:
--------
Integer array representing the number of artifacts in each tomb.

Output Format:
--------
Single Integer representing the maximum number of artifacts you can collect without disturbing any two adjacent tombs on the same night

Sample Input-1:
--------
```
2 4 3
```

Sample Output-1:
--------
```
4   
```

Explanation:
--------
You cannot loot tomb 1 (artifacts = 2) and tomb 3 (artifacts = 3), as they are adjacent in a circular setup.


Sample Input-2:  
--------
```
1 2 3 1
```

Sample Output-2:
--------
```
4
```

Explanation:
--------
You can loot tomb 1 (1 artifact) and tomb 3 (3 artifacts) without breaking the ancient rule.  
Total = 1 + 3 = 4 artifacts.


Sample Input-3:  
--------
```
1 2 3
```

Sample Output-3:
--------
```
3 
```

Constraints:  
--------
-  1 <= artifacts.length <= 100 
-  0 <= artifacts\[i] <= 1000 

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] main) {
    Scanner sc = new Scanner(System.in);
    
    int artifacts[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    
    System.out.println(maxCollectable(artifacts));
    
    sc.close();
  }
  
  private static int maxCollectable(int[] artifacts) {
    if (artifacts.length==1) return artifacts[0];
    
    int n = artifacts.length;
    
    return Math.max(collect(artifacts, 0, n-1, new int[n]), collect(artifacts, 1, n, new int[n]));
  }
  
  private static int collect(int[] artifacts, int s, int e, int[] dp) {
    if (s>=e) return 0;
    
    if (dp[s]!=0) return dp[s];
    
    int p = artifacts[s] + collect(artifacts, s+2, e, dp);
    int np = collect(artifacts, s+1, e, dp);
    
    return dp[s] = Math.max(p, np);
  }
}
```