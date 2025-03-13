#Coding/TwoPointer  

There are floods in the eastern India.There are infinite number of boats available with National Disaster Response force.Where each boat can carry a maximum weight limit.

Each boat carries at most two people at same time provided the sum of those people is at most limit. 

Return the minimum number of boats to carry every given person to rescue them.
 
Input Format
------------
Line1: Two space separated integers, representing no of people and limit of boat
Line2: space separated integers represents weight of each person 

Output Format
-------------
An integer represents minimum no of boats required

Sample Input-1: 
-----------
```
2 3
1 2
```

Sample Output:
------------
```
1
```

Explanation:
------------
1 boat (1, 2)


Sample Input-2:
------------
```
4 3
3 2 2 1
```

Sample Output-2:
------------
```
3
```

Explanation:
------------
3 boats (1, 2), (2) and (3)


Sample Input-3:
------------
```
4 5
3 5 3 4
```

Sample Output-3:
------------
```
4
```

Explanation:
------------
4 boats (3), (3), (4), (5)

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String args[]) {
    Scanner sc = new Scanner(System.in);
    
    int n = sc.nextInt(), lim = sc.nextInt();

    int arr[] = new int[n];
    for (int i=0; i<n; i++) arr[i] = sc.nextInt();
    
    System.out.println(countBoats(arr, lim));

    sc.close();
  }

  private static int countBoats(int[] arr, int lim) {
    Arrays.sort(arr);

    int l = 0, r = arr.length-1, c = 0;
    
    while (l<=r) {
      if (arr[l]+arr[r] <= lim) l++;
      r--; c++;
    }

    return c;
  }
}
```