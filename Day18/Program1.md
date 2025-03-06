#Coding/DisjointSetUnion 

Budget Padmanabham planned to visit the tourist places. There are N tourist places in the city. The tourist places are numbered from 1 to N. 

You are given the routes[] to travel between the tourist places in the city where routes\[i]=\[place-1, place-2, price], A route is a bi-directional route. You can travel from place-1 to place-2 or place-2 to place-1 with the given price.
 
Your task is to help Budget Padmanabham to find the cheapest route plan, to visit all the tourist places in the city. If you are not able to find such plan, print -1.
 
Input Format:
-------------
Line-1: Two space separated integers N and R,number of places and routes.
Next R lines: Three space separated integers, start, end and price.
  
Output Format:
--------------
Print an integer, minimum cost to visit all the tourist places.
 
 
Sample Input-1:
---------------
```
4 5
1 2 3
1 3 5
2 3 4
3 4 1
2 4 5
```
 
Sample Output-1:
----------------
```
8
```
 
Explanation:
------------
The cheapest route plan is as follows:
1-2, 2-3, 3-4 and cost is 3 + 4 + 1 = 8
 
 
Sample Input-2:
---------------
```
4 3
1 2 3
1 3 5
2 3 4
```
 
Sample Output-2:
----------------
```
-1
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt(), l = sc.nextInt();

    int[][] routes = new int[l][3];
    for (int i = 0; i < l; i++) {
      routes[i][0] = sc.nextInt();
      routes[i][1] = sc.nextInt();
      routes[i][2] = sc.nextInt();
    } 

    System.out.println(cheapestRoute(routes, n));
   
    sc.close();
  }
  
  private static int cheapestRoute(int[][] routes, int n) {
    Arrays.sort(routes, (a, b) -> (a[2] - b[2]));

    int[] parent = new int[n+1];
    for (int i=0; i<n; i++) parent[i] = i;

    int edges=0, cost=0;
    
    for (int[] curr: routes)
      if (union(curr[0], curr[1], parent)) {
        edges++;
        cost += curr[2];
      }
    
    return edges==n-1 ? cost : -1;
  }
  
  private static int find(int x, int[] parent) {
    if (parent[x]!=x) parent[x] = find(parent[x], parent);
    return parent[x];
  }

  private static boolean union(int x, int y, int[] parent) {
    int rootX = find(x, parent), rootY = find(y, parent);
    
    if (rootX==rootY) return false;

    if (rootX<rootY) parent[rootX] = rootY;
    else parent[rootY] = rootX;

    return true;
  }
}
```