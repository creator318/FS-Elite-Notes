#Coding/DFS

Imagine you're an adventurer with a mystical treasure map. This map is a grid of ancient runes, where each cell holds a single character. Legend has a powerful incantation—represented as a string—is hidden within these runes. To unlock the treasure, you must verify if the incantation exists on the map.

The incantation is formed by linking runes that are directly next to each other either horizontally or vertically. Each rune on the map can only be used once in the incantation.

Your Task:  
Given an m x n grid representing the treasure map and a string representing the incantation, return true if the incantation can be traced on the map; otherwise, return false.


Sample Input-1:
----------
```
3 4
ABCD
SFCS
ADEE
ABCCED
```

Sample Output-1:
----------
```
ABCCED can be traced
```

Explanation:
----------
Treasure Map Grid:  
```
A B C E
S F C S
A D E E
```
Incantation: "ABCCED" exists in map

Sample Inpu-2:
----------
```
3 4
ABCE
SFCS
ADEE
ABCB
```

Sample Output-2:
----------
```
ABCB cannot be traced
```

Explanation:
----------
Treasure Map Grid:  
```
A B C E
S F C S
A D E E
```
Incantation: "ABCB" does not exist in map


Constraints:
----------
- m == the number of rows in the grid  
- n == the number of columns in the grid  
- 1 <= m, n <= 6  
- 1 <= incantation length <= 15  
- The grid and incantation consist only of uppercase and lowercase English letters.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt(), m = sc.nextInt();
    
    char[][] map = new char[n][m];
    for (int i=0; i<n; i++)
      map[i] = sc.next().toCharArray();

    String path = sc.next();
    
    System.out.println(path + (pathPresent(map, path) ? " can " : " cannot ") + "be traced");
    
    sc.close();
  }
  
  private static boolean pathPresent(char[][] map, String path) {
    boolean visited[][] = new boolean[map.length][map[0].length];
    
    for (int i=0; i<map.length; i++) {
      for (int j=0; j<map[0].length; j++) {
        if (map[i][j]==path.charAt(0)) {
          if (dfs(map, path, visited, i, j, 1)) return true;
        }
      }
    }
    
    return false;
  }
  
  private static boolean dfs(char[][] map, String path, boolean[][] visited, int r, int c, int pos) {
    if (pos==path.length()) return true;
    
    int dirs[][] = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    
    visited[r][c] = true;
    for (int[] dir: dirs) {
      int nr = r+dir[0], nc = c+dir[1];
      if (nr>=0 && nr<map.length && nc>=0 && nc<map[0].length && !visited[nr][nc] && map[nr][nc]==path.charAt(pos))
        if (dfs(map, path, visited, nr, nc, pos+1)) return true;
    }
    
    return false;
  }
}
```