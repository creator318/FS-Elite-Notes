#Coding/BackTracking 

Bablu is working in a construction field.
He has N number of building blocks, where the height and width of all the blocks are same and the length of each block is given in an array, blocks\[].

Bablu is planned to build a wall in the form of a square. The rules to cunstruct the wall are as follows:
- He should use all the building blocks.
- He should not break any building block, but you can attach them with other.
- Each building-block must be used only once.
	
Your task is to check whether Bablu can cunstruct the wall as a square with the given rules or not. If possible, print true. Otherwise, print false.

Input Format:
-------------
Line-1: An integer N, number of BuildingBlocks.
Line-2: N space separated integers, length of each block.

Output Format:
--------------
Print a boolean value.

Sample Input-1:
---------------
```
6
1 2 6 4 5 6
```

Sample Output-1:
----------------
```
true
```


Sample Input-2:
---------------
```
6
5 3 2 5 5 6
```

Sample Output-2:
----------------
```
false
```

## Solution

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt();
    int lens[] = new int[n];
    for (int i=0; i<n; i++) lens[i] = sc.nextInt();

    System.out.println(checkIfSquare(lens));
    
    sc.close();
  }

  public static boolean checkIfSquare(int[] lens) {
    int sum = Arrays.stream(lens).sum();

    if (sum%4!=0 || lens.length<4) return false;
    return backtrack(lens, sum/4, 0, new int[] {0});
  }

  public static boolean backtrack(int[] lens, int side, int curr, int[] count) {
    if (count[0]==4) {
      for (int x: lens)
        if (x>0) return false;

      return true;
    }

    if (curr==side) return true;
    if (curr>side) return false;

    for (int i=0; i<lens.length; i++) {
      if (lens[i]>0) {
        lens[i] *= -1;
        if (backtrack(lens, side, curr+lens[i], count)) {
          count[0]++;
          return true;
        }
        lens[i] *= -1;
     }
    }
    return false;
  }
}
```