#Coding/BackTracking 

A shipping company is managing two cargo ships: a large cargo ship and a smaller cargo ship. The company has divided the large cargo ship into X compartments and the smaller cargo ship into Y compartments, where X > Y.

Each compartment in both ships is loaded with a specific amount of cargo. 
The company needs to relocate cargo from the large cargo ship to the smaller cargo ship by selecting Y compartments from the large ship and transferring their cargo to Y compartments in the smaller ship, maintaining the respective order.

However, due to weight balance regulations, the amount in compartment n+1 should always be greater than or equal to that in the compartment n of the smaller cargo ship, after the transferred from the large cargo ship.

Your task is to help the company determine the number of ways they can transfer the cargo while satisfying these regulations.

Input Format:
-------------
Number of compartments in the large cargo ship (X).
Number of compartments in the smaller cargo ship (Y).
Cargo weights in compartments of the large cargo ship.
Cargo weights in compartments of the smaller cargo ship.

Output Format:
----------------
Return the number of valid ways to transfer the cargo.


Sample Input-1:
--------------
```
5
3
1 5 2 4 7
7 8 6
```

Sample Output-1:
----------------
```
6
```

Explanation:
-----------
The compartments from the large cargo ship can be selected as:
- (1, 2, 7)
- (1, 2, 4)
- (1, 4, 7)
- (1, 5, 7)
- (5, 4, 7)
- (2, 4, 7)  

Thus, there are 6 valid ways to transfer the cargo.

Sample Input-2:
--------------
```
4
2
7 7 7 7
3 4
```

Sample Output-2:
----------------
```
6
```

Explanation:
-----------
The compartments from the large cargo ship can be selected as:
- (1st, 2nd) (7,7)
- (1st, 3rd) (7,7)
- (1st, 4th) (7,7)
- (2nd, 3rd) (7,7)
- (2nd, 4th) (7,7)
- (3rd, 4th) (7,7)  

Thus, there are 6 valid ways to transfer the cargo.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String args[]) {
    Scanner sc = new Scanner(System.in);
    
    int x = sc.nextInt();
    int y = sc.nextInt();
    
    int X[] = new int[x];
    for (int i=0; i<x; i++) X[i] = sc.nextInt();
    
    int Y[] = new int[y];
    for (int i=0; i<y; i++) Y[i] = sc.nextInt();
    
    System.out.println(numWays(X, Y));
    
    sc.close();
  }
  
  private static int numWays(int[] X, int[] Y) {
    int res[] = { 0 };
    for (int i=0; i<X.length; i++) {
      int last = X[i] + Y[0];
      backtrack(X, Y, res, i+1, 1, 1, last);
    }
    return res[0];
  }

  private static void backtrack(int[] X, int Y[], int[] res, int ix, int iy, int count, int last) {
    if (count==Y.length) {
      res[0]++;
      return;
    }

    if (ix>=X.length || iy>=Y.length) {
      if ((count) == Y.length) res[0]++;
      return;
    }
    
    for (int i=ix; i<X.length; i++) {
      if (last<=Y[iy]+X[i]) {
        backtrack(X, Y, res, i+1, iy+1, count+1, Y[iy]+X[i]);
      }
    }
  }
}
```