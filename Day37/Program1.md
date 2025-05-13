#Coding/Backtracking 

In a forgotten realm, explorers often find ancient treasure maps written as long strings of mysterious characters with no spaces. Luckily, they also carry an ancient wordbook (pathBook) containing all the known names of places, landmarks, and directions.

Your task is to help the explorer decode the map by inserting spaces such that each segment is a valid location or direction from the pathBook. Return all possible ways to break the map string into a valid sequence of known locations.

You can reuse entries from the pathBook as many times as needed.

Input Format:
----------
Line-1: A single string representing the treasure map 
Line-2: Space separated list of strings representing the pathBook

Output Format:
----------
Array of strings representing the sequence of valid locations

Sample Input-1:
----------
```
deserttemplegolds
desert temple gold golds
```

Sample Output-1:
----------
```
[desert temple gold]
```

Explanation:
----------
The map can be decoded directly into three known places.

Sample Input-2:
----------
```
forestforesthill
forest hill
```

Sample Output-2:
----------
```
[forest forest hill]
```

Explanation:
----------
The explorer can reuse 'forest' more than once.

Sample Input-3:
----------
```
oceanmountaintemple
mountain temple
```

Sample Output-3:
----------
```
[]
```

Explanation: 
----------
The map begins with 'ocean', which is missing from the pathBook, so no decoding is possible.

Map Decoding Constraints:
----------
- 1 <= map.length <= 20
- 1 <= pathBook.length <= 1000
- 1 <= pathBook\[i].length <= 10
- All strings consist of lowercase English letters.
- All entries in pathBook are unique.
- Input is structured so the total number of valid decoded strings does not exceed $10^5$.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    String map = sc.next();
    sc.nextLine();
    String[] pathBook = sc.nextLine().split(" ");

    System.out.println(findPath(map, pathBook));

    sc.close();
  }

  private static List<String> findPath(String map, String[] pathBook) {
    Set<String> locations = new HashSet<>(Arrays.asList(pathBook));
    List<String> path = new LinkedList<>();

    backtrack(map, locations, 0, path, new LinkedList<>());

    return path;
  }

  private static void backtrack(String map, Set<String> locations, int pos, List<String> path, List<String> curr) {
    if (pos==map.length()) {
      path.addAll(curr);
      return;
    }

    for (int i=pos; i<map.length(); i++) {
      String subs = map.substring(pos, i+1);
      if (locations.contains(subs)) {
        curr.add(subs);
        backtrack(map, locations, i+1, path, curr);
        curr.remove(curr.size()-1);
      }
    }
  }
}
```