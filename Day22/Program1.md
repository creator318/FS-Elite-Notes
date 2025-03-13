#Coding/TopologicalSort 

Imagine you are designing a treasure hunt that takes participants through n unique landmarks, each identified by a distinct number from 1 to n. 
The official treasure map provides an itinerary—called the route—which is a complete ordering (a permutation) of these landmarks. Along the way, m clues are given in the form of several smaller landmark sequences. Each clue is a pair of landmarks that appear in the route in the same order, though not necessarily consecutively.

Your challenge is to determine whether the provided route is both the shortest possible itinerary that incorporates all the clues and the only such itinerary. 
The shortest itinerary is defined as one that contains every clue as an ordered subsequence while including no extra landmarks beyond what is necessary. Although there might be multiple itineraries that include all clues, the route must be unique and minimal for your treasure hunt to be considered well-defined.

Display valid if the official route is the only shortest itinerary that fits all the clues, or invalid otherwise.


Input format:
-------------
Line 1: n m, space separated 2 integers
Line 2: n space separated integers, representing official route
Line 3: m lines of clues, each clue has 2 space separated integers

Output format:
---------------
Official route is valid or official route is invalid.


Sample Input-1:
-------------
```
3 2
1 3 2
1 2
1 3
```

Sample Output-1:
-------------
```
[1, 3, 2] is invalid
```

Explanation: 
-------------
- There are two possible itineries: \[1,2,3] and \[1,3,2].
- The clue \[1,2] is a subsequence of both: \[1,2,3] and \[1,3,2].
- The clue \[1,3] is a subsequence of both: \[1,2,3] and \[1,3,2].
- Since there are multiple valid shortest itineraries, the official route is not unique.

Sample Input-2:
-------------
```
3 1
1 2 3
1 2
```

Sample Output-2:
-------------
```
[1, 2, 3] is invalid
```

Explanation: 
-------------
- The shortest possible itinerary is \[1,2].
- The clue \[1,2] is a subsequence of it: \[1,2].
- Since official route is not the shortest itinerary, we return false.

Sample Input-3:
-------------
```
3 3
1 2 3
1 2
1 3
2 3
```

Sample Output-3:
-------------
```
[1, 2, 3] is valid
```
  
Explanation: 
-------------
- The only possible shortest itinerary that contains all the clues is \[1, 2, 3]. 
- No alternative minimal route exists. 

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt(), m = sc.nextInt();

    int path[] = new int[n];
    for (int i=0; i<n; i++) path[i] = sc.nextInt();
    
    int paths[][] = new int[m][2];
    for (int i=0; i<m; i++) {
      paths[i][0] = sc.nextInt();
      paths[i][1] = sc.nextInt();
    }
    
    System.out.println(Arrays.toString(path) + " is " + (isValidPath(n, m, path, paths) ? "valid" : "invalid"));

    sc.close();
  }
  
  private static boolean isValidPath(int n, int m, int[] path, int[][] paths) {
    List<List<Integer>> graph = new LinkedList<>();
    int[] indegree = new int[n + 1];
    
    for (int i=0; i<=n; i++) graph.add(new LinkedList<>());
    for (int[] p: paths) {
      graph.get(p[0]).add(p[1]);
      indegree[p[1]]++;
    }
    
    return isValid(graph, indegree, path);
  }
  
  private static boolean isValid(List<List<Integer>> graph, int[] indegree, int[] path) {
    Queue<Integer> q = new LinkedList<>();

    for (int i=1; i<indegree.length; i++) if (indegree[i]==0) q.add(i);
    
    List<Integer> check = new ArrayList<>();
    while (!q.isEmpty()) {
      if (q.size() > 1) return false;

      int curr = q.poll();
      for (int next: graph.get(curr)) 
        if (--indegree[next] == 0) 
          q.add(next);
      
      check.add(curr);
    }
    
    return Arrays.equals(path, check.stream().mapToInt(i -> i).toArray());
  }
}
```