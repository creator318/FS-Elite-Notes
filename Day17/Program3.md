#Coding/Intuition

A grid of light bulbs is given, represented as a matrix of size rows x cols, 
where each cell contains either 0 (off) or 1 (on).

Your task is to turn all the light bulbs off (0) by following the toggle rule:
    - In each step, you can choose either an entire row or an entire column and toggle all its elements (change 0 to 1 and 1 to 0).
    
At the end, if all light bulbs are turned off, print true, otherwise print false.

Input Format
-------------
Line-1: Read two integers rows and cols(space separated).
Line-2: Read the matrix of dimension rows X cols.

Output Format
--------------
Print a boolean result.


Sample input-1:
---------------
```
5 5
0 0 1 0 0
0 0 1 0 0
1 1 0 1 1
0 0 1 0 0
0 0 1 0 0
```

Sample output-1:
----------------
```
true
```

Explanation:
------------
```
0 0 1 0 0          0 0 1 0 0           0 0 0 0 0
0 0 1 0 0   row-3  0 0 1 0 0   cols-3  0 0 0 0 0
1 1 0 1 1   --->   0 0 1 0 0   --->    0 0 0 0 0
0 0 1 0 0          0 0 1 0 0           0 0 0 0 0
0 0 1 0 0          0 0 1 0 0           0 0 0 0 0 
```


Sample input-2
--------------
```
2 2
1 1
0 1
```

Sample output-2
---------------
```
false
```


### Intuition : All rows are identical or any row is opposite of rest, then true

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt(), m = sc.nextInt();

    boolean[][] grid = new boolean[n][m];
    for (int i = 0; i < n; i++)
      for (int j = 0; j < m; j++)
        grid[i][j] = sc.nextInt()==1 ? true : false;

    System.out.println(canToggle(grid));

    sc.close();
  }

  public static boolean isSwitchedOn(boolean[][] grid) {
    for (int i=0; i<grid.length; i++)
      for (int j=0; j<grid[0].length; j++)
        if (grid[i][j]) return false;

    return true;
  }

  public static boolean canToggle(boolean[][] grid) {
    for (int i=0; i<grid.length; i++)
      if (grid[i][0])
        for (int j=0; j<grid[0].length; j++)
          grid[i][j] = !grid[i][j];

    for (int i = 0; i < grid[0].length; i++)
      if (grid[0][i])
        for (int j = 0; j < grid.length; j++)
          grid[j][i] = !grid[j][i];

    return isSwitchedOn(grid);
  }
}
```