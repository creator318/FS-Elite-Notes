#Coding/DisjointSetUnion 

There are N cities, and M routes\[], each route is a path between two cities.
Routes\[i] = \[city 1, city 2], there is a travel route between city 1 and city 2.
Each city is numbered from 0 to N-1.
 
There are one or more Regions formed among N cities. 
A Region is formed in such way that you can travel between any two cities 
In the region that are connected directly and indirectly.
 
Your task is to find out the number of regions formed between N cities. 
 
Input Format:
-------------
Line-1: Two space separated integers N and M, number of cities and routes
Next M lines: Two space separated integers city 1, city 2.
 
Output Format:
--------------
Print an integer, number of regions formed.
 
 
Sample Input-1:
---------------
5 4
0 1
0 2
1 2
3 4
 
Sample Output-1:
----------------
2
 
 
Sample Input-2:
---------------
5 6
0 1
0 2
2 3
1 2
1 4
2 4
 
Sample Output-2:
----------------
1
 
## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt(), m = sc.nextInt();
  
    int[][] grid = new int[m][m];
    for (int i=0; i<m; i++) {
      grid[i][0] = sc.nextInt();
      grid[i][1] = sc.nextInt();
    }
  
    System.out.println(countRegions(grid, n, m));
    sc.close();
  }
  
  private static int countRegions(int[][] grid, int n, int m) {
    int[] parent = new int[n];
    for (int i=0; i<n; i++) parent[i] = i;

    int[] rank = new int[n];
    for (int i=0; i<m; i++) union(grid[i][0], grid[i][1], parent, rank);

    Set<Integer> set = new HashSet<>();
    for (int i=0; i<n; i++) set.add(find(i, parent));
    return set.size();
  }

  private static int find(int x, int[] parent) {
    if (parent[x]!=x) parent[x] = find(parent[x], parent);
    return parent[x];
  }  

  private static void union(int x, int y, int[] parent, int[] rank) {
    int rootX = find(x, parent), rootY = find(y, parent);

    if (rootX == rootY) return;
    
    if (rank[rootX]<rank[rootY]) parent[rootX] = rootY;
    else if (rank[rootX]>rank[rootY]) parent[rootY] = rootX;
    else {
      parent[rootX] = rootY;
      rank[rootY]++;
    }  
  }  
}
```