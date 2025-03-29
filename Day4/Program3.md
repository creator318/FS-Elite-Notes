#Coding/SlidingWindow 

You are a renowned Alchemist exploring a mystical forest teeming with magical plants. 
Each plant you encounter has a unique Essence Power represented by an integer in 
the array plants of length n.

By collecting essences from consecutive plants, you can brew powerful Elixirs. 
The potency of an elixir is determined by the sum of the essence powers of the plants used. 
Due to the complexity of brewing, you can only create elixirs using sequences of 
Plants that are at least m in length.

Your objective is to find the kth smallest elixir potency that can be brewed 
From these sequences.

Input Format:
-------------
Line-1: 3 space separated integers, n, k, m
Line-2: n space separated integers, movieRatings[].

Output Format:
-------------
An integer, The kth smallest elixir potency

Sample Input-1:
--------
```
3 2 2
2 1 3
```

Sample Output-1:
--------
```
4
```

Explanation:
--------
Possible elixirs (sequences of length ≥ 2):
- \[2, 1]: Potency = 2 + 1 = 3
- \[1, 3]: Potency = 1 + 3 = 4
- \[2, 1, 3]: Potency = 2 + 1 + 3 = 6

Ordered elixir potencies: 3, 4, 6
The 2 nd smallest elixir potency is 4.

Sample Input-2:
--------
```
4 3 2
3 -3 5 2
```

Sample output-2:
--------
```
4
```

Explanation:
--------
Possible elixirs (sequences of length ≥ 2):
- \[3, -3]: Potency = 3 + -3 = 0
- \[-3, 5]: Potency = -3 + 5 = 2
- \[5, 2]: Potency = 5 + 2 = 7
- \[3, -3, 5]: Potency = 3 + -3 + 5 = 5
- \[-3, 5, 2]: Potency = -3 + 5 + 2 = 4
- \[3, -3, 5, 2]: Potency = 3 + -3 + 5 + 2 =7

Ordered elixir potencies: 0, 2, 4, 5, 7, 7
The 3 rd smallest elixir potency is 4.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
        
    int n=sc.nextInt(), k=sc.nextInt(), m=sc.nextInt();
    int[] arr = new int[n];
    for (int i=0; i<n; i++) arr[i] = sc.nextInt();
        
    System.out.println(slide(arr, k, m));
        
    sc.close();
  }
    
  private static int slide(int[] arr, int k, int m) {
    Queue<Integer> q = new PriorityQueue<>();
        
    int curr = 0;
    for (int i=0; i<m; i++) {
      curr += arr[i];
    }
        
    for (int i=0; i<=arr.length-m; i++) {
      int currIter = curr;
      for (int j=i+m-1; j<arr.length; j++) {
        q.add(currIter);
        if (j<arr.length-1) currIter += arr[j+1];
      }
            
      if (i==arr.length-m) continue;
        curr += arr[i+m] - arr[i];
    }

    int res=0;
    for (int i=0; i<k; i++) res=q.remove();
    return res;
  }
}
```