#Coding/SlidingWindow

You are given an integer array nums and two integers l and r. Your task is to 
Find the minimum positive energy produced by a sequence of operations. 
Each operation corresponds to selecting a contiguous subarray of nums 
Whose length is between l and r (inclusive).

The energy of a sequence is defined as the product of all the numbers in 
The subarray. You need to find the sequence with the smallest positive energy.

If no such sequence exists, return -1.

Input Format:
-------------------
Line-1: Three space separated integers, N, L and R.
Line-2: N space separated integers, array[].

Output Format:
-------------------
An integer result, smallest positive energy.

Sample Input-1:
-------------------
```
W
4 2 3
2 -1 3 4
```

Sample Output-1:
-------------------
```
12
```

Explanation:
-------------------
The possible sequences of operations with lengths between l = 2 and r = 3 are:

\[2, -1] (not valid, energy = -2)
\[3, 4] (energy = 12)
\[2, -1, 3] (not valid, energy = -6)
The sequence \[3, 4] produces the smallest positive energy of 12. Hence, 
The output is 12.

Sample Input-2:
-------------------
```
3 2 3
1 -3 2
```

Sample Output-1:
-------------------
```
-1
```

Explanation:
-------------------
No valid sequence produces a positive energy. Thus, the output is -1.

Constraints:
============
1 ≤ nums. Length ≤ 100
1 ≤ l ≤ r ≤ nums. Length
−1000 ≤ nums\[i] ≤ 1000


## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
        
    int n = sc.nextInt();
    int l = sc.nextInt(), r = sc.nextInt();
        
    int arr[] = new int[n];
    for (int i=0; i<n; i++) arr[i] = sc.nextInt();
        
    System.out.println(solve(arr, l, r));
        
    sc.close();
  }
    
  private static long solve(int[] arr, int l, int r) {
    long min = Long.MAX_VALUE;
    
    for (int i=l; i<=r; i++) {
    	long p=1;
    	int prev=0, cz=0;
    	for (int j=0; j<i; j++) {
        if (arr[j]==0) cz++;
        else p *= arr[j];
    	}

    	if (p>0 && cz==0) min = Math.min(p, min);

    	for(int j=i; j<arr.length; j++){
        if (arr[prev] != 0) p /= arr[prev];
        else cz--;

        if (arr[j]==0) cz++;
        else p *= arr[j];
                
        if (p>0 && cz==0) min = Math.min(p, min);
        prev += 1;
    	}
    }
    return min == Long.MAX_VALUE ? -1 : min;
  }
}
```