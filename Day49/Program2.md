Pranav has a puzzle board filled with square boxes in the form of a grid. Some cells in the grid may be empty. '0' - indicates empty, '1' - indicates a box. 

Pranav wants to find out the number of empty spaces which are completely surrounded by the square boxes (left, right, top, bottom) in the board.

You are given the board in the form of a grid M*N, filled wth 0's and 1's. Your task is to help Pranav to find the number of empty groups surrounded by the boxes in the puzzle board.

Input Format:
-------------
Line-1: Two integers M and N, the number of rows and columns in the board.
Next M lines: contains N space-separated either 0 or 1.

Output Format:
--------------
Print an integer, the number of empty spaces in the puzzle board.


Sample Input-1:
---------------
```
6 7
1 1 1 1 0 0 1
1 0 0 0 1 1 0
1 0 0 0 1 1 0
0 1 1 1 0 1 0
1 1 1 0 0 1 1
1 1 1 1 1 1 1
```

Sample Output-1:
----------------
```
2
```

Explanation:
------------
The 2 empty groups are as follows:
1st group starts at cell(1,1), 2nd group starts at cell(3,4).
The groups which are starts at cell(0,4), cell(1,6) and cell(3,0) are not valid empty groups, because they are not completely surrounded by boxes.


Sample Input-2:
---------------
```
6 6
1 1 0 0 1 1
1 0 1 1 0 1
0 1 0 1 0 0
1 1 0 0 0 1
0 0 1 0 1 1
1 1 0 1 0 0
```

Sample Output-2:
----------------
```
1
```

Explanation:
------------
The only empty group starts at cell(1,1) is surrounded by boxes.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
  
    int m = sc.nextInt(), n = sc.nextInt();
    
    int grid[][] = new int[m][n];
    for (int i = 0; i < m; i++)
      for (int j = 0; j < n; j++)
        grid[i][j] = sc.nextInt();

    System.out.println(countEmpty(grid));

    sc.close();
  }

  private static int countEmpty(int[][] grid) {
    int m = grid.length, n = grid[0].length, res = 0;

    for (int i=0; i<m; i++)
      for (int j=0; j<n; j++)
        if (grid[i][j]==0) 
          res += bfs(grid, i, j);
    
    return res;
  }

  private static int bfs(int[][] grid, int i, int j) {
    int m = grid.length, n = grid[0].length;
    if (i==0 || j==0 || i==m-1 || j==n-1) return 0;

    Queue<int[]> q = new LinkedList<>();
    q.offer(new int[] { i, j });

    int nRow[] = { -1, 0, 1, 0 }, nCol[] = { 0, 1, 0, -1 };
    grid[i][j] = 1;

    boolean check = true;
    while (!q.isEmpty()) {
      int[] curr = q.poll();
      int r = curr[0], c = curr[1];

      if (r==0 || c==0 || r==m-1 || c==n-1) check = false;
  
      for (int x=0; x<4; x++) {
        int nr = r + nRow[x], nc = c + nCol[x];
        
        if (nr>=0 && nr<m && nc>=0 && nc<n && grid[nr][nc]==0) {
          q.offer(new int[] { nr, nc });
          grid[nr][nc] = 1;
        }
      }
    }
    
    return check ? 1 : 0;
  }
}
```