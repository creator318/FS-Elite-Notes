#Coding/Greedy 

A group of researchers is analyzing satellite imagery of agricultural fields, represented by a grid of land sections. Each section is marked either as fertile (1) or infertile (0). To efficiently plan crop planting, the researchers need to identify the largest rectangular area consisting exclusively of fertile land sections.

Given a binary grid representing the land (1 for fertile and 0 for infertile), your task is to compute the area of the largest rectangle consisting entirely of fertile land sections.

Input Format:
-------------
Line-1: Two integers rows(r) and columns(c) of grid.
Next r lines: c space separated integers, each row of the grid.

Output Format:
--------------
Print an integer, area of the largest rectangle consisting entirely of fertile land sections.

Sample Input-1:
--------
```
4 5
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

Sample Output-1:
--------
```
6
```

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

    System.out.println(maxRectArea(grid));

    sc.close();
  }

  private static int maxRectArea(int[][] grid) {
    int m = grid.length, n = grid[0].length; 
    int[][] prefixSum = new int[m+1][n];

    for (int i=0; i<m; i++)
      for (int j=0; j<n; j++)
        prefixSum[i+1][j] += prefixSum[i][j] + grid[i][j];

    int max = Integer.MIN_VALUE;
    for (int i=0; i<m; i++) {
      for (int j=i+1; j<=m; j++) {
        int curMax = 0, cur = 0;
        for (int k=0; k<n; k++) {
          if ((prefixSum[j][k]-prefixSum[i][k])==(j-i)) cur++;
          else cur = 0;

          curMax = Math.max(curMax, cur);
        }

        max = Math.max(max, curMax * (j-i));
      }
    }
    
    return max;
  }
}
```