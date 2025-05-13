#Coding/Greedy 

You are controlling a battlefield represented by an m x n grid. 
Each cell on this grid can be one of the following:
- A reinforced barrier ('B'), blocking your laser.
- An enemy drone ('D'), your target.
- An open cell ('0'), where you can position your robot to fire.

When your robot fires its powerful laser from an open cell, the beam destroys all enemy drones in the same row and column until the beam hits a barrier ('B'). The barrier completely stops the laser, protecting anything behind it.

Your goal is to identify the best position (open cell) to place your robot so that firing the laser destroys the maximum number of enemy drones in a single shot. Return this maximum number.

Input format:
-------------
Line 1: Two space separated integers, represents m & n
Next M lines: each row of battlefield


Output format:
--------------
An integer, maximum number of enemy drones destroyed in a single shot.

Sample Input-1:
----------
```
3 4
0 D 0 0
D 0 B D
0 D 0 0
```

Sample Output-1:
----------
```
3
```

Explanation: 
----------
Placing robot at battlefield\[1]\[1] destroys 3 enemy drones in a single shot.

Sample Input-2:
----------
```
3 3
B B B
0 0 0
D D D
```

Sample Output-2:
----------
```
1
```

Constraints:
----------
- 1 <= m, n <= 500
- battlefield\[i]\[j] is either 'B', 'D', or '0'.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main (String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int r = sc.nextInt(), c = sc.nextInt();
    
    char grid[][] = new char[r][c];
    for (int i=0; i<r; i++)
      for (int j=0; j<c; j++)
        grid[i][j] = sc.next().charAt(0);
        
    System.out.println(maxTargets(grid));
    
    sc.close();
  }
  
  private static int maxTargets(char[][] grid) {
    int r = grid.length, c = grid[0].length, res = -1;
    int vals[][] = new int[r][c];


    int up[] = new int[c], down[] = new int[c]; 
    for (int i=0; i<r; i++) {
      int left = 0, right = 0;
      for (int j=0; j<c; j++) {
        if (grid[i][j]=='D') { left++; up[j]++; }
        else if (grid[i][j]=='B') left = up[j] = 0; 
        else vals[i][j] += left + up[j];

        int rj = c-1-j;
        if (grid[i][rj]=='D') right++;
        else if (grid[i][rj]=='B') right = 0;
        else vals[i][rj] += right;
    
        int ri = r-1-i;
        if (grid[ri][j]=='D') down[j]++;
        else if (grid[ri][j]=='B') down[j] = 0;
        else res = Math.max(res, vals[ri][j] += down[j]);
      }
    }
    return res;
  }
}
```