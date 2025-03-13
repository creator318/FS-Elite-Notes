#Coding/TopologicalSort

Imagine you're the master chef in a renowned kitchen, tasked with preparing a spectacular dinner consisting of numDishes unique dishes, labeled from 0 to numDishes - 1. However, the recipes have dependencies—certain dishes can only be prepared after completing others. You’re given a list called dependecies, where each element dependencies\[i] = \[$X_i$, $Y_i$] means that you must finish preparing dish $Y_i$ before starting dish $X_i$.

For instance, the pair \[2, 1] implies that to create dish 2, dish 1 must be prepared first.

Return the ordering of dishes that a chef should take to finish all dishes.
	- the result set should follow the given order of conditions.
	- If it is impossible to complete all dishes, return an empty set.

Input Format:
-------------
Line-1: An integer numDishes, number of Dishes.
Line-2: An integer m, number of dependencies.
Next m lines: Two space separated integers, $X_i$ and $Y_i$.

Output Format:
--------------
Return a list of integers, the ordering of dishes that a chef should finish.

Sample Input-1:
------------
```
4
3
1 2
3 0
0 1
```

Sample Output-1:
------------
```
[2, 1, 0, 3]
```


Explanation:
------------
There are 4 dishes. Since dish 1 requires dish 2, dish 3 requires dish 0 and dish 0 requires dish 1, you can prepare dishes in the order 2 1 0 3.


Sample Input-2:
----------
```
2
2
1 0
0 1
```

Sample Output-2:
------------
```
[]
```

Explanation:
------------
There are 2 dishes, but dish 1 depends on dish 0 and dish 0 depends on dish 1. This circular dependency makes it impossible to prepare all dishes.

Constraints:
------------
- 1 <= numDishes <= 2000  
- 0 <= m <= 5000  
- dependencies[i].length == 2  
- 0 <= $X_i , Y_i$ < numDishes  
- All the dependency pairs are unique.

## Solution: 

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt(), d = sc.nextInt();

    int dishes[][] = new int[d][2];
    for (int i=0; i<d; i++) {
      dishes[i][0] = sc.nextInt();
      dishes[i][1] = sc.nextInt();
    }

    System.out.println(orderDishes(dishes, n));

    sc.close();
  }

  private static List<Integer> orderDishes(int[][] dishes, int n) {
    List<List<Integer>> graph = new LinkedList<>();

    for (int i=0; i<n; i++) graph.add(new LinkedList<>());
    for (int d[]: dishes) graph.get(d[1]).add(d[0]);

    return topologicalSort(graph);
  }

  private static List<Integer> topologicalSort(List<List<Integer>> graph) {
    Set<Integer> visited = new HashSet<>();
    Stack<Integer> stk = new Stack<>();

    for (int i=0; i<graph.size(); i++)
      if (! visited.contains(i))
        dfs(graph, i, visited, stk);

    List<Integer> res = new LinkedList<>();
    for (int i=0; i<graph.size(); i++) res.add(stk.pop());
    
    return res;
  }

  private static void dfs(List<List<Integer>> graph, int v, Set<Integer> visited, Stack<Integer> stk) {
    visited.add(v);
    for (int i: graph.get(v))
      if (!visited.contains(i))
        dfs(graph, i, visited, stk);
    stk.push(v);
  }
}
```