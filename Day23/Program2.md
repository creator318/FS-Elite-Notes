#Coding/Graphs/Representation

Imagine you're playing an adventure video game with a total of 'n' quests, numbered from 0 to n–1. Some quests have special unlock conditions: certain quests must be completed before others can be attempted. You’re provided with 'm' questDependencies where each element questDependencies\[i] = \[$a_i$, $b_i$] means that you must finish quest $a_i$ before you can start quest $b_i$.

For instance, the dependency\[0, 1] implies that quest 0 must be cleared before quest 1 becomes available. These requirements can also be indirect; if quest 'a' unlocks quest 'b', and quest 'b' unlocks quest 'c', then quest 'a' is considered an indirect prerequisite for quest 'c'.

In addition, you receive an array called inquiries of size 'k' where each inquiry inquiries\[j] = \[$u_j$, $v_j$] asks whether quest $u_j$ is a necessary precursor to quest $v_j$. Your task is to return a boolean array result where each result\[j] answers the $\text{j}^\text{th}$ inquiry.

Input format:
-------------
Line 1: Three space separated integers, representing n, m and k
Line 2: next m lines of dependencies
Line 3: next k lines of queries

Output format:
--------------
Resultant boolean array.

Sample Input-1:
----------
```
2 1 2
1 0
0 1
1 0
```
  
Sample Output-1: 
----------
```
[false, true]
```

Explanation:
----------
The dependency \[1, 0] indicates that you must complete quest 1 before attempting quest 0. Hence, quest 0 is not a prerequisite for quest 1, but quest 1 is for quest 0.

Sample Input-2:
----------
```
2 0 2
1 0
0 1
```

Sample Output-2:
----------
```
[false, false]
```
  
Explanation:
----------
With no dependencies, each quest stands alone.

Sample Input-3:
----------
```
3 2 2
1 2
2 0
1 0
1 2
```

Sample Output-3:
----------
```
[true, true]
```

Constraints:
----------
- 2 <= n <= 100  
- 0 <= m <= (n * (n - 1) / 2)  
- Each questDependencies\[i] contains exactly 2 elements  
- 0 <= $a_i$ , $b_i$ <= n - 1, with $a_i$ != $b_i$  
- All pairs \[$a_i$, $b_i$] are unique  
- The dependency structure does not contain cycles  
- 1 <= k <= $10^4$ 
- 0 <= $u_j$, $v_j$ <= numMissions - 1

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt(), m = sc.nextInt(), k=sc.nextInt();
    
    int deps[][] = new int[m][2];
    for (int i=0; i<m; i++) {
      deps[i][0] = sc.nextInt();
      deps[i][1] = sc.nextInt();
    }
    
    int queries[][] = new int[k][2];
    for (int i=0; i<k; i++) {
      queries[i][0] = sc.nextInt();
      queries[i][1] = sc.nextInt();
    }
    
    System.out.println(solveDeps(deps, n, queries));

    sc.close();
  }
  
  private static List<Boolean> solveDeps(int[][] deps, int n, int[][] queries) {
    List<Set<Integer>> graph = new LinkedList<>();
    
    for (int i=0; i<=n; i++) graph.add(new HashSet<>());
    for (int[] d: deps) graph.get(d[0]).add(d[1]);
    
    List<Boolean> res = new LinkedList<>();
    for (int[] q: queries) res.add(isDependency(graph, q));
    return res;
  }
  
  private static boolean isDependency(List<Set<Integer>> graph, int[] query) {
    if (graph.get(query[0]).contains(query[1])) return true;

    for (int i: graph.get(query[0]))
      if (isDependency(graph, new int[]{i,query[1]}))
        return true;

    return false;        
  }
}
```
