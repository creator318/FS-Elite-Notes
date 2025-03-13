#Coding/SlidingWindow 

Imagine you're a botanist managing a long row of garden plots. Each plot is 
planted with a flower that has a unique color code represented by an integer. 
You are given an array garden of length n, where each element denotes the color 
of the flower in that plot, and an integer k which indicates the number of 
consecutive plots you will examine at a time. Your objective is to determine 
how many different flower colors appear in every contiguous block of k plots.

Return an array result where result\[i] is the count of unique flower colors in 
the section of the garden from plot i to plot i + k - 1.

## Input Format:

Line-1: N  
Line-2: N space separated integers, representing flowers in each garden plot
Line-3: K

## Output Format:

Array of ints representing no of flower in each garden plot blobk.

Sample Input-1: 
------
```
7
1 2 3 2 2 1 3
3
```

Sample Output-1:
------
```
[3, 2, 2, 2, 3]
```

Explanation:
------
For plots 0 to 2: \[1,2,3] → 3 unique colors
For plots 1 to 3: \[2,3,2] → 2 unique colors
For plots 2 to 4: \[3,2,2] → 2 unique colors
For plots 3 to 5: \[2,2,1] → 2 unique colors
For plots 4 to 6: \[2,1,3] → 3 unique colors

Sample Input-2:
------
```
7
1 1 1 1 2 3 4
4
```

Sample Output-2:
------
```
[1, 2, 3, 4]
```

Explanation:
------
For plots 0 to 3: \[1,1,1,1] → 1 unique color
For plots 1 to 4: \[1,1,1,2] → 2 unique colors
For plots 2 to 5: \[1,1,2,3] → 3 unique colors
For plots 3 to 6: \[1,2,3,4] → 4 unique colors

Constraints:
------
- 1 <= k <= garden.length <= $10^5$
- 1 <= garden\[i] <= $10^5$

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt();
    int flowers[] = new int[n];
    for (int i=0; i<n; i++) flowers[i] = sc.nextInt();
    int k = sc.nextInt();
    
    System.out.println(blockFlowerCounts(flowers, k));    

    sc.close();
  }
  
  private static List<Integer> blockFlowerCounts(int[] flowers, int k) {
      Map<Integer, Integer> map = new HashMap<>();
      List<Integer> res = new LinkedList<>();
      
      int c=0;
      for (int i=0; i<k; i++) { 
        map.put(flowers[i], map.getOrDefault(flowers[i], 0)+1);
        if (map.get(flowers[i])==1) c++;  
      }
      
      for (int i=0; i<=flowers.length-k; i++) {
        res.add(c);
        
        if (i==flowers.length-k) continue;
        map.put(flowers[i], map.getOrDefault(flowers[i], 1)-1);
        if (map.get(flowers[i])==0) c--;
        map.put(flowers[i+k], map.getOrDefault(flowers[i+k], 0)+1);
        if (map.get(flowers[i+k])==1) c++;
      }
      
      return res;
  }
}
```