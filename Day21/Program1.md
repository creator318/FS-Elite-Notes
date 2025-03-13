#Coding/TopologicalSort 

Imagine you're hosting a mystery treasure hunt with n puzzles numbered from 1 to n. 
Some puzzles have hidden clues that can only be uncovered after solving certain earlier puzzles. You’re provided with m number of dependencies, where each dependency is given as \[earlierPuzzle, laterPuzzle]—meaning you must solve the earlierPuzzle before attempting laterPuzzle. Additionally, you can work on at most k puzzles in a single day, but only if you’ve already solved all the prerequisite puzzles on previous days.

Determine the minimum number of days required to solve all the puzzles. It is guaranteed that the dependencies allow every puzzle to eventually be solved.


Input format:
----------
Line 1: 3 space separated integers n m & k
Line 2: m lines of dependencies

Output format:
----------
An integer, minimum number of days.

Sample Input-1:
----------
```
4 3 2
2 1
3 1
1 4
```

Sample Output-1:
----------
```
3
```

Explanation:
----------
- On Day 1, you solve puzzles 2 and 3.  
- On Day 2, having unlocked the clue, you solve puzzle 1.  
- On Day 3, you solve puzzle 4.

Sample Input-2:
----------
```
5 4 2
2 1
3 1
4 1
1 5
```

Sample Output-2:
----------
```
4
```

Explanation:
----------
- On Day 1, you can solve puzzles 2 and 3 (you can’t solve more than two in a day).  
- On Day 2, you solve puzzle 4.  
- On Day 3, you solve puzzle 1.  
- On Day 4, you finally solve puzzle 5.

Constraints:
----------
- 1 <= n <= 15  
- 1 <= k <= n  
- 0 <= m <= n * (n-1) / 2  
- Each dependency has exactly two elements  
- 1 <= earlierPuzzle, laterPuzzle <= n  
- earlierPuzzle != laterPuzzle  
- All dependency pairs are unique  
- The dependency graph forms a directed acyclic graph

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt(), m = sc.nextInt(), k = sc.nextInt();
    
    int deps[][] = new int[m][2];
    for (int i=0; i<m; i++) {
      deps[i][0] = sc.nextInt();
      deps[i][1] = sc.nextInt();
    }

    System.out.println(minDays(deps, n, k));

    sc.close();
  }

  private static int minDays(int[][] deps, int n, int k) {
    List<List<Integer>> graph = new LinkedList<>();
    int[] indegree = new int[n + 1];

    for (int i=0; i<=n; i++) graph.add(new LinkedList<>());
    for (int d[]: deps) {
      graph.get(d[0]).add(d[1]);
      indegree[d[1]]++;
    }

    return findMinDays(graph, indegree, k);
  }

  private static int findMinDays(List<List<Integer>> graph, int[] indegree, int k) {
    Queue<Integer> q = new LinkedList<>();

    for (int i=1; i<indegree.length; i++) 
      if (indegree[i]==0)
        q.add(i);
    
    int res = 0;

    while (!q.isEmpty()) {
      int lim = Math.min(q.size(), k);
      for (int i=0; i<lim; i++) {
        int curr = q.poll();
        for (int next: graph.get(curr))
          if (--indegree[next]==0)
            q.add(next);
      }
      res++;
    }

    return res;
  }
}
```