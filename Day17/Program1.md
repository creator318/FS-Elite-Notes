#Coding/DisjointSetUnion 

There are N computers in a network, all the computers are connected as tree structure. And one new connection is added in the Network. The computers in the network are identified with their IDs, the IDs are numbered between 1 to N.

The connections in the network is given as connection\[i] = \[comp-A, comp-B], there is a connection between comp-A and comp-B.

Your task is to remove a connection in the network and print it, so that  all the computers are connected as tree structure. If there are multiple options to remove, remove the connection that occurs last in the input.


Input Format:
-------------
Line-1: Two space separated integers N, number of computers.
Next N lines: Two space separated integers, comp-A & comp-B.

Output Format:
--------------
Print the connection which is removed.


Sample Input-1:
---------------
```
6
1 2
3 4
3 6
4 5
5 6
2 3
```

Sample Output-1:
---------------
```
5 6
```


Sample Input-2:
---------------
```
4
1 2
2 3
3 4
2 4
```

Sample Output-2:
---------------
```
2 4
```


## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt();
    
    int[][] grid = new int[n][2];
    for (int i=0; i<n; i++) {
      grid[i][0] = sc.nextInt();
      grid[i][1] = sc.nextInt();
    }

    System.out.println(removalChoice(grid));
    
    sc.close();
  }

  private static List<Integer> removalChoice(int[][] grid) {
    int[] parent = new int[grid.length+1];
    for (int i=0; i<=grid.length; i++) parent[i] = i;

    int[] rank = new int[grid.length+1];
    int[] res = new int[3];
    
    for (int i=0; i<grid.length; i++) union(grid[i][0], grid[i][1], parent, rank, res);
    
    return Arrays.asList(res[1], res[2]);
  }
  
  private static int find(int x, int[] parent) {
    if (parent[x]!=x) parent[x] = find(parent[x], parent);
    return parent[x];
  }

  private static void union(int x, int y, int[] parent, int[] rank, int[] res) {
    int rootX = find(x, parent), rootY = find(y, parent);

    if (rootX==rootY) {
      if (res[0]!=-1) 
        parent[res[1]] = parent[res[2]] = res[0];

      res[0] = rootX;
      res[1] = x;
      res[2] = y;
      return;
    }

    if (rank[rootX]<rank[rootY]) parent[rootX] = rootY;
    else if (rank[rootX]>rank[rootY]) parent[rootY] = rootX;
    else {
      parent[rootX] = rootY;
      rank[rootY]++;
    }
  }
}
```