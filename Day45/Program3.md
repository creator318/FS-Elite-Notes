#Coding/Graphs/ArticulationPoints

In a faraway galaxy, interstellar explorers have mapped N planets, numbered from 0 to N-1, interconnected through space routes represented by the given 'routes'. Each element routes[i] = [ai, bi] denotes a direct space route between planet 'ai' and planet 'bi'.

A Critical Gateway Planet (also known as an articulation point) is a special planet whose removal (along with its space routes) increases the number of disconnected Galactic Regions, thereby isolating groups of planets from each other.

Given the number of planets (N), number of routes (M) and a list of direct space routes (routes), identify and list all the Critical Gateway Planets within this galaxy.

Input Format:
-------------
Line-1: Two space separated integers N and M, number of planets and routes
Next M lines: Two space separated integers ai and bi.
 
Output Format:
--------------
Print an integer, number of disconnected Galactic Regions.

Sample Input-1:
----------
```
5 5
0 1
1 2
2 0
1 3
3 4
```

Sample Output-1:
----------
```
1 3
```

Explanation:
----------
Removing planet 1 disconnects the galaxy into two separate regions: {0,2} and {3,4}.
Removing planet 3 isolates planet 4, increasing the number of Galactic Regions.


Sample Input 2:
-----------
```
4 3
0 1
1 2
2 3
```

Sample Output-2:
----------
```
1 2
```

Explanation:
----------
Removing planet 1 or 2 increases the Galactic Regions from 1 to 2.


Constraints:
----------
- 1 <= n <= 2000
- 1 <= routes.length <= 5000
- routes\[i].length == 2
- 0 <= $a_i$ <= $b_i$ < n
- $a_i$ != $b_i$
- No repeated space routes exist (routes).

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int p = sc.nextInt(), r = sc.nextInt();

    int routes[][] = new int[r][2];
    for (int i=0; i<r; i++)
      routes[i] = new int[] {sc.nextInt(), sc.nextInt()};

    System.out.println(articulationPoints(routes, p));
   
    sc.close();
  }

  private static List<Integer> articulationPoints(int[][] edges, int v) {
    List<List<Integer>> adj = buildAdjList(edges, v);

    return getArticulationPoints(adj);
  }

  private static List<List<Integer>> buildAdjList(int[][] edges, int v) {
    List<List<Integer>> adj = new LinkedList<>();
    for (int i=0; i<v; i++)
      adj.add(new LinkedList<>());
      
    for (int e[]: edges) {
      adj.get(e[0]).add(e[1]);
      adj.get(e[1]).add(e[0]);
    }

    return adj;
  }

  private static List<Integer> getArticulationPoints(List<List<Integer>> adj) {
    int v = adj.size();

    int dTime[] = new int[v], low[] = new int[v], parent[] = new int[v];
    boolean visited[] = new boolean[v];
    
    Set<Integer> res = new TreeSet<>();  

    Arrays.fill(parent, -1);
    for (int i=0; i<v; i++)
      if (!visited[i])
        APUtil(adj, i, visited, dTime, low, parent, res, new int[] {0});

    return new LinkedList<>(res);
  }

  private static void APUtil(List<List<Integer>> adj, int cur, boolean[] visited, int[] dTime, int[] minDTime, int[] parent, Set<Integer> res, int[] time) {
    visited[cur] = true;
    dTime[cur] = minDTime[cur] = ++time[0];

    int children = 0;
    for (int i: adj.get(cur)) {
      if (!visited[i]) {
        children++;
        parent[i] = cur;
        APUtil(adj, i, visited, dTime, minDTime, parent, res, time);

        minDTime[cur] = Math.min(minDTime[cur], minDTime[i]);

        if ((parent[cur]==-1 && children>1) || (parent[cur]!=-1 && minDTime[i]>=dTime[cur]))
          res.add(cur);
      } else if (i!=parent[cur])
        minDTime[cur] = Math.min(minDTime[cur], dTime[i]);
    }
  }
}
```