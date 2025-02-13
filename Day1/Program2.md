#Coding/SlidingWindow  

The Kingdom of CodeLand has a long bridge made of square tiles, 
Where each tile is painted either red ('R') or blue ('B'). 
A critical mission has been assigned to you: ensure that a section of the 
Bridge is sturdy enough to support heavy traffic. Blue tiles are reinforced,
While red tiles are weak and need to be reinforced by painting them blue.

You are given a 0-indexed string bridge of length n, where bridge[i] represents the color of the i-th tile. Each character in bridge is either 'R' (red) or 'B' (blue).

Your goal is to ensure that there is at least one segment of k consecutive blue tiles on the bridge to support heavy traffic. You can repaint a red tile ('R') to a blue tile ('B') in one operation.

Write a program to determine the minimum number of repaint operations needed to create a segment of k consecutive blue tiles.

Input Format:
---------------
Space separated string and integer, S and K

Output Format:
-----------------
An integer value.


Sample Input-1:
------------------
```
RRBRBBRRBR 4
```

Sample Output-1:
--------------------
```
1
```

Explanation:
-------------
One way to achieve 4 consecutive blue tiles is to repaint the 4 th tile to get bridge = "RRBBBBRRBR".
This requires 1 operations.

Sample Input-2:
------------------
```
BRBRRBB 3
```

Sample Output-2:
--------------------
```
1
```

Explanation:
--------------
One way to achieve 3 consecutive blue tiles is to repaint the 2 nd tile to get bridge = "BBBRRBB".
This requires only 1 operation.

Sample Input-3:
------------------
```
BBBRRBRBRR 5
```

Sample Output-3:
--------------------
```
2
```

Explanation:
--------------
One way to achieve 5 consecutive blue tiles is to repaint the 4 th and 9 th tiles to get bridge = "BBBBBBBRRR".
This requires 2 operations.


## Solution:

```java
import java.util.*;

public class Solution {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        
        String s = sc.next();
        int n = sc.nextInt();

        System.out.println(slide(s, n));
        
        sc.close();
    }
    
    private static int slide(String s, int n) {
        int c=n;
        for (int i=0; i<n; i++) if (s.charAt(i) == 'B') c--;
        
        int min = c;
        for (int i=0; i<s.length()-n; i++) {
            c += s.charAt(i) == 'B' ? 1 : 0;
            c -= s.charAt(i+n) == 'B' ? 1 : 0;
            
            min = Math.min(c, min);
        }
        return min;
    }
}
```