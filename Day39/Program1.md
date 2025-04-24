#Coding/DynamicProgramming 

You are the chief designer for a futuristic garden trail, where there are N consecutive decorative light posts along a path. Each post can be illuminated with one of three distinct colors: Crimson, Azure, or Emerald. 
The cost to illuminate each post with a particular color depends on the post's location and the energy circuit required.

However, to maintain visual presentation and prevent power circuit overloads, no two adjacent posts can have the same light color.

The cost of illuminating each post with each color is provided in a 2D array costs, where costs\[i]\[0] is the cost of lighting post i with Crimson, costs\[i]\[1] with Azure, and costs\[i]\[2] with Emerald.

Your goal is to find the minimum total cost to light up all posts in such a way that no two neighboring posts have the same color.

Input Format:
-------------
Line-1: An integer N, number of post.
Next N lines: 3 space separated integers, cost of illuminating the posts.

Output Format:
--------------
Print an integer, minimum total cost to light up all posts.

Sample Input:
---------------
3
17 2 17
16 4 5
14 3 19

Sample Output:
----------------
10

Explanation: 
------------
1st post is with Azure, 2nd post is with Emerald,
3rd post is with Crimson.
Minimum cost: 2 + 5 + 3 = 10.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt();
    
    int costs[][] = new int[n][3];
    for (int i=0; i<n; i++)
      costs[i] = new int[] {sc.nextInt(), sc.nextInt(), sc.nextInt()};
    
    System.out.println(minLightingCost(costs));
    
    sc.close();
  }
  
  private static int minLightingCost(int[][] costs) {
    int dp[][] = new int[costs.length][3];
    
    int min = Integer.MAX_VALUE;
    for (int i=0; i<3; i++)
      min = Math.min(min, minCost(costs, 0, i, dp));
      
    return min;
  }
  
  private static int minCost(int[][] costs, int pos, int curr, int[][] dp) {
    if (pos==costs.length) return 0;
    
    if (dp[pos][curr]!=0) return dp[pos][curr];
    
    int min = Integer.MAX_VALUE;
    
    for (int col=0; col<3; col++)
      if (col!=curr)
        min = Math.min(min, minCost(costs, pos+1, col, dp));
    
    return dp[pos][curr] = costs[pos][curr] + min;
  }
}
```