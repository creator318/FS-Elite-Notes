#Coding/DynamicProgramming 

You are navigating a spaceship through a galaxy represented as an m x n space map. The spaceship starts from the top-left sector (sector\[0]\[0]) and its mission is to safely reach the bottom-right sector (sector\[m-1]\[n-1]).

Each sector on the map can either be clear (0) or blocked by an asteroid field (1). The spaceship can only move right or downward at any moment. It cannot pass through sectors with asteroid fields.

Find the total number of distinct safe routes the spaceship can take to reach its destination at the bottom-right corner.


Input format:
-------------
2 space seperated integers, m & n
next m lines representing the Map 

Output format:
--------------
An integer, the total number of distinct safe routes


Sample Input-1:
----------
```
3 3
0 0 0
0 1 0
0 0 0
```

Sample Output-2:
----------
```
2
```

Explanation:  
----------
There’s one asteroid field blocking the center of the 3×3 map.  
Two possible safe navigation paths:
- Move → Move → Down → Down
- Down → Down → Move → Move

Sample Input-2:
---------
```
2 2
0 1
0 0
```

Sample Output-2:
----------
```
1
```


Constraints:
----------
- m == sectorMap.length
- n == sectorMap\[i].length
- 1 <= m, n <= 100
- sectorMap\[i]\[j] is either 0 (clear) or 1 (asteroid field)

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int r = sc.nextInt(), c = sc.nextInt();

    int grid[][] = new int[r][c];
    for (int i=0; i<r; i++) 
      for (int j=0; j<c; j++)
        grid[i][j] = sc.nextInt();

    System.out.println(countPaths(grid));

    sc.close();
  }

  private static int countPaths(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    
    if (grid[m-1][n-1]==1 || grid[0][0]==1) return 0;
    
    int dp[][] = new int[m][n];
    dp[m-1][n-1] = 1;

    for (int i=m-1; i>0; i--)
      dp[i-1][n-1] = (grid[i-1][n-1]==0 && dp[i][n-1]==1) ? 1 : 0;

    for (int j=n-1; j>0; j--)
      dp[m-1][j-1] = (grid[m-1][j-1]==0 && dp[m-1][j]==1) ? 1 : 0;

    for (int i=m-1; i>0; i--) {
      for (int j=n-1; j>0; j--) {
        if (grid[i-1][j-1]==0)
          dp[i-1][j-1] = dp[i][j-1] + dp[i-1][j];
        else
          dp[i-1][j-1] = 0;
      }
    }

    return dp[0][0];
  }
}
```