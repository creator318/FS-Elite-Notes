#Coding/TwoPointer  

A Kid built a structure using building blocks, by placing the building-blocks adjacent to each other.

A building-block is a vertical alignment of blocks.
here one block each represents  as \[ ] .

The following structure made up of using building blocks

```
           [ ]
       [ ] [ ]     [ ]
       [ ] [ ] [ ] [ ] [ ]             [ ]
   [ ] [ ] [ ] [ ] [ ] [ ]     [ ]     [ ]
0   1   3   4   2   3   2   0   1   0   2
```
Once the structure is completed, kid pour water(w) on it.

You are given a list of integers, heights of each building-block in a row.
Now your task How much amount of water can be stored by the structure.

 Input Format:
 -------------
 Space separated integers, heights of the blocks in the structure.

Output Format:
 --------------
 Print an integer,

Sample Input:
-------------
```
 0 1 3 4 2 3 2 0 1 0 2
```

Sample Output:
--------------
```
6
```

Explanation:
 -----------
In the above structure,  6 units of water (w represents the water in the structure)
can be stored.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
   
    int blocks[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
   
    System.out.println(getWater(blocks));
    
    sc.close();
  }
  
  public static int getWater(int[] blocks) {
      int n = blocks.length;
      if (n==0) return 0;

      int[] lm = new int[n], rm = new int[n];
      lm[0] = blocks[0];
      rm[n-1] = blocks[n-1];

      for (int i=1; i<n; i++)
        lm[i] = Math.max(lm[i-1], blocks[i]);

      for (int i=n-2; i>=0; i--) 
        rm[i] = Math.max(rm[i+1], blocks[i]);

      int trappedWater = 0;
      for (int i=0; i<n; i++)
        trappedWater += Math.min(lm[i], rm[i]) - blocks[i];

      return trappedWater;
  }
}
```