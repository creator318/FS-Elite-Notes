#Coding/BFS #Coding/DFS 

Pranav has a puzzle board filled with square boxes in the form of a grid. Some cells in the grid may be empty. '0' - indicates empty, '1' - indicates a box. 

The puzzle board has some patterns formed with boxes in it, the patterns may be repeated. The patterns are formed with boxes (1's) only, that are connected horizontally and vertically but not diagonally.

Pranav wants to find out the number of unique patterns in the board.

You are given the board in the form of a grid M*N, filled wth 0's and 1's.
Your task is to help Pranav to find the number of unique patterns in the puzzle board.

Input Format:
-------------
Line-1: Two integers M and N, the number of rows and columns in the grid-land.
Next M lines: contains N space-separated integers \[0, 1].

Output Format:
--------------
Print an integer, the number of unique patterns in the puzzle board.


Sample Input-1:
---------------
```
5 5
0 1 0 1 1
1 1 1 0 1
0 1 0 1 0
1 0 1 1 1
1 1 0 1 0
```

Sample Output-1:
----------------
```
3
```

Explanation-1:
------------
The unique patterns are as follows:
```
  1	     1 1      1 
1 1 1      1    1 1
  1	
```
   
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
5
```

Explanation-2:
------------
The unique patterns are as follows:
```
1 1    1 1    1      1 1    1
1        1    1 1 		
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt(), m = sc.nextInt();

    boolean[][] grid = new boolean[n][m];
    for (int i=0; i<n; i++)
      for (int j=0; j<m; j++)
        grid[i][j] = sc.nextInt()==1 ? true : false;

    System.out.println(getUniquePatterns(grid));

    sc.close();
  }

  private static int getUniquePatterns(boolean[][] grid) {
    Set<String> set = new HashSet<>();
    Queue<int[]> queue = new LinkedList<>();

    for (int i=0; i<grid.length; i++)
      for (int j=0; j<grid[0].length; j++)
        if (grid[i][j]) {
          queue.offer(new int[] { i, j });
          grid[i][j] = false;
          set.add(bfs(grid, queue));
        }
    return set.size();
  }

  private static String bfs(boolean[][] grid, Queue<int[]> queue) {
    StringBuilder sb = new StringBuilder();

    int[] nRows = { -1, 0, 1, 0 }, nCols = { 0, 1, 0, -1 };
    char[] direction = { 'l', 'u', 'r', 'd' };

    while (!queue.isEmpty()) {
      int[] top = queue.poll();
      int r = top[0], c = top[1];

      for (int i=0; i<4; i++) {
        int nr = r + nRows[i], nc = c + nCols[i];
        if (nr>=0 && nr<grid.length && nc>=0 && nc<grid[0].length && grid[nr][nc]) {
          queue.offer(new int[] { nr, nc });
          sb.append(direction[i]);
          grid[nr][nc] = false;
        } else sb.append('x');
      }
    }
    return sb.toString();
  }
}
```