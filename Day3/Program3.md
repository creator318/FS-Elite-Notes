#Coding/SlidingWindow 

You are given an integer array nums and two integers l and r. Your task is to 
Analyze the volatility of a sequence of values. The volatility of a sequence is 
Defined as the difference between the maximum and minimum values in that sequence.

You need to determine the sequence with the highest volatility among all 
Sequences of length between l and r (inclusive).

Return the highest volatility. If no such sequence exists, return -1.

Input Format:
-------------
Line-1: 3 space separated integers, n, l, r
Line-2: n space separated integers, nums\[].

Output Format:
-------------
An integer, the highest volatility.


Sample Input-1:
---------------
```
5 2 3
8 3 1 6 2
```
Sample Output-1:
----------------
```
7
```

Explanation:
------------
The possible sequences of length between l = 2 and r = 3 are:

\[8, 3] with a volatility of 8−3=5
\[3, 1] with a volatility of 3−1=2
\[1, 6] with a volatility of 6−1=5
\[8, 3, 1] with a volatility of 8−1=7
The sequence \[8, 3, 1] has the highest volatility of 7.

Sample Input-2:
---------------
```
4 2 4
5 5 5 5
```

Sample Output-2:
----------------
```
0
```

Explanation:
------------
All possible sequences have no volatility as the maximum and minimum values 
Are the same, resulting in a difference of 0.
 
## Solution:

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner (System.in);
        
        int n = sc.nextInt(), l = sc.nextInt(), r = sc.nextInt();
        int arr[] = new int[n];
        for (int i=0; i<n; i++) arr[i] = sc.nextInt();
        
        System.out.println(slide(arr, l, r));
        
        sc.close();
    }
    
    private static int slide(int arr[], int l, int r) {
        int score = Integer.MIN_VALUE;
        
        Queue<Integer> min = new PriorityQueue<>();
        Queue<Integer> max = new PriorityQueue<>(Collections.reverseOrder());
        
        // Max volatility will only be for the largest array size
        for (int i=0; i<r; i++) {
            min.add(arr[i]);
            max.add(arr[i]);
        }
        
        for (int i=0; i<=arr.length-r; i++) {
            score = Math.max(score, max.peek() - min.peek());
        
            if (i==arr.length-r) continue;
            
            if (max.peek()==arr[i]) max.remove();
            max.add(arr[i+r]);
            if (min.peek()==arr[i]) min.remove();
            min.add(arr[i+r]);
        }
        
        return score;
    }
}
```