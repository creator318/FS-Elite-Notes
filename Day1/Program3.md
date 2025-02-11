#Coding  #Coding/SlidingWindow  

You are an architect tasked with designing a series of connected rooms in a 
Building. You are given a list of room sizes represented by an integer array 
RoomSizes, an integer maxFrequency representing the maximum number of times 
A particular room size can appear, and an integer maxArea representing 
The maximum allowable total area of connected rooms. A set of connected 
Rooms is called viable if the frequency of each room size in this set is 
Less than or equal to maxFrequency, and the total area of the rooms in 
This set is less than or equal to maxArea. 

Return the length of the longest viable set of connected rooms from roomSizes.

Input Format:
-------------
Line-1: 3 space separated integers, n, maxFrequency, maxArea
Line-2: N space separated integers, roomSizes[].

Output Format:
-------------
An integer, the length of the longest viable set of connected rooms


Sample Input-1:
---------------
```
8 2 10
1 2 3 1 2 3 1 2
```

Sample Output-1:
----------------
```
5
```

Explanation: 
------------
The longest possible viable set of connected rooms is \[1, 2, 3, 1, 2] since 
The room sizes 1, 2, and 3 appear at most twice, and the total area is less than 10.

Sample Input-2:
---------------
```
6 1 3
1 2 1 2 1 2
```

Sample Output-2:
----------------
```
2
```

Explanation: The longest possible viable set of connected rooms is \[1, 2] since 
The room sizes 1 and 2 appear at most once, and the total area is 3.

Constraints:
------------
1 <= roomSizes. Length <= 10^5
1 <= roomSizes\[i] <= 10^4 where roomSizes\[i] is the size of the i-th room.
1 <= maxFrequency <= roomSizes. Length
1 <= maxArea <= 10^9

## Solution: 

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int n = sc.nextInt(), mf = sc.nextInt(), ma = sc.nextInt();
        int[] arr = new int[n];
        
        for (int i=0; i<n; i++) arr[i] = sc.nextInt();
        
        System.out.println(slide(arr, mf, ma));
        
        sc.close();
    }
    
    private static int slide(int[] arr, int mf, int ma) {
        Map<Integer, Integer> map = new HashMap<>();
        
        int a=0, l=0, r=0, max=0;
        while (r<arr.length) {
            map.put(arr[r], map.getOrDefault(arr[r], 0) + 1);
            a += arr[r];
            while ((map.get(arr[r]) > mf || a>ma) && l<=r) {
                map.put(arr[l], map.get(arr[l]) - 1);
                a -= arr[l++];
            }
            r++;
            max = Math.max(max, r-l);
        }
        
        return max;
    }
}
```