#Coding/SlidingWindow 

You are an architect designing a street with houses represented as a 0-indexed array 
House_heights of n integers, where each element represents the height of a house. 
Additionally, a binary array visibility_mask of length n is provided, where 1 indicates 
A house that contributes to the neighborhood's skyline, and 0 indicates a house that does not.

For any house located at index i, you are tasked with determining the average 
skyline height within a k-radius neighborhood centered at i. The average skyline 
Height is the average of all house heights between indices i - k and i + k (inclusive) 
That have a corresponding visibility value of 1 in the visibility_mask. 

If no houses with a visibility of 1 exist in the range, or if the range extends 
Beyond the boundaries of the array, the average skyline height for that house is -1.

Return an array skyline_avgs of length n, where skyline_avgs\[i] is the average 
skyline height for the neighborhood centered at index i.

Input Format:
--------
Line 1: n and k
Next 2 Lines: n space separated integers each, representing heights and visibilities respectively

Sample Input- 1:
--------
```
7 2
10 15 20 5 30 25 40
1 0 1 1 0 1 1
```

Sample Output-1:
--------
```
-1 -1 11 16 22 -1 -1
```

Explanation:
--------
- For index 0, there are less than k houses in the left neighborhood, 
  So skyline_avgs\[0] = -1.
- For index 1, there are less than k houses in the left neighborhood, 
  So skyline_avgs\[1] = -1.
- For index 2, the neighborhood is \[10, 15, 20, 5, 30]. From the visibility_mask, 
  Only the houses with heights \[10, 20, 5] contribute to the skyline. The average 
  Is (10 + 20 + 5) / 3 = 11.
- For index 3, the neighborhood is \[15, 20, 5, 30, 25]. From the visibility_mask, 
  Only the houses with heights \[20, 5, 25] contribute. 
  The average is (20 + 5 + 25) / 3 = 16.
- For index 4, the neighborhood is \[20, 5, 30, 25, 40]. From the visibility_mask, 
  Only the houses with heights \[20, 5, 25, 40] contribute. The average 
  Is (20 + 5 + 25 + 40) / 4 = 22.
- For index 5, there are less than k houses in the right neighborhood, 
  So skyline_avgs\[5] = -1.
- For index 6, there are less than k houses in the right neighborhood, 
  So skyline_avgs\[6] = -1.

Sample Input-2:
--------
```
3 1
50 60 70
1 1 1
```

Sample Output-2:
--------
```
-1 60 -1
```

Constraints:
========
1. N == house_heights. Length == visibility_mask. Length
2. 1 <= n <= $10^5$
3. 0 <= house_heights\[i] <= $10^5$
4. Visibility_mask\[i] is either 0 or 1
5. 0 <= k <= n

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
        
    int n = sc.nextInt(), k = sc.nextInt();
        
    int[] heights = new int[n];
        
    for (int i=0; i<n; i++) heights[i] = sc.nextInt();
        
    boolean[] visibility = new boolean[n];
    for (int i=0; i<n; i++) visibility[i] = sc.nextInt() == 1;
        
    for (int i: slide(heights, visibility, k)) System.out.print(i + " ");
    System.out.println();
        
    sc.close();
  }
    
  private static int[] slide(int[] heights, boolean[] visibility, int k) {
    int[] res = new int[heights.length];
    Arrays.fill(res, -1);
        
    int curr=0, size=0;
    for (int i=0; i<=Math.min(2*k, heights.length-1); i++) {
      if (visibility[i]) {
        curr+=heights[i];
        size++;
      }
    }
        
    for (int i=k; i<heights.length-k; i++) {
      if (size != 0) res[i] = (int)curr / size;
            
      if (i==heights.length-k-1) continue;
      if (visibility[i-k]) {
        curr -= heights[i-k];
        size--;
      }
      if (visibility[i+k+1]) {
        curr += heights[i+k+1];
        size++;
      }
    }
        
    return res;
  }
}
```