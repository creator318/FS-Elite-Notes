#Coding/DynamicProgramming 

Your playing a game, the game contains m*n grid.
Each cell in the grid represent the health points.
You can move one step either downwads or rightwards only.
Whenever you visit a cell in the grid, you will lose the same health points of the cell.

You will start at (0,0) cell of the grid and have to reach (m-1)*(n-1) cell.
Your task is to minimize the loss of overall health points.

Input Format:
-------------
Line-1: Two integers M and N.
Next M lines: N space separated integers, health points in each row of the grid.

Output Format:
--------------
Print an integer, minimal loss of overall health points.

Sample Input-1:
---------------
```
3 3
1 3 1
1 5 1
4 2 1
```

Sample Output-1:
----------------
```
7
```

Explanation:
------------
start at cell(0,0) =>  1→3→1→1→1 minimizes the loss of health points.


Sample Input-2:
---------------
```
4 4
8 6 9 3
7 6 2 1
5 5 7 9
8 9 6 2
```

Sample Output-2:
----------------
```
34
```

Explanation:
------------
start at cell(0,0) =>  8→6→6→2→1→9→2 minimizes the loss of health points.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int m = sc.nextInt(), n = sc.nextInt();

    int[][] grid = new int[m][n];
    for (int i=0; i<m; i++) 
      for (int j=0; j<n; j++) 
        grid[i][j] = sc.nextInt();

    System.out.println(minHealthPoints(grid, m, n));
    
    sc.close();
  }

  private static int minHealthPoints(int[][] grid, int m, int n) {
    int[][] dp = new int[m][n];
    dp[0][0] = grid[0][0];
    
    for (int i=1; i<m; i++)
      dp[i][0] = grid[i][0] + dp[i-1][0];

    for (int i=1; i<n; i++)
      dp[0][i] = grid[0][i] + dp[0][i-1];

    for (int i=1; i<m; i++)
      for (int j=1; j<n; j++)
        dp[i][j] = grid[i][j] + Math.min(dp[i-1][j], dp[i][j-1]);

    return dp[m-1][n-1];
  }
}
```