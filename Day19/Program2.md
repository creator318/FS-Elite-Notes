#Coding/BitManipulation

Mr. Athreya is a beginner in computer programming. He is given an integer D, a decimal value. 
Your task is to count the 1's in the BCD form of each integer N and store the count in an array of size D+1, where N is in the range of  0<= N <= D.
 
**Example of BCD:**
Given an integer I=10 , The BCD of I is '1010', and 1's count is 2.
 
NOTE: Can you solve it in O(D) time?
 
Input Format:
-------------
An integer D, the decimal value.
 
Output Format:
--------------
Print the array of D+1 integers.

Sample Input-1:
---------------
```
3
```
 
Sample Output-1:
----------------
```
0 1 1 2
```

Sample Input-2:
---------------
```
6
```
 
Sample Output-2:
----------------
```
0 1 1 2 1 2 2
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt();
    
    System.out.println(bitCounts(n));
    
    sc.close();
  }

  private static List<Integer> bitCounts(int n) {
    List<Integer> res = new LinkedList<>();

    for (int i=0; i<=n; i++) {
      int c=1, curr=i;
      while ((curr &= curr-1)>0) c++;
      res.add(Math.min(c,i));
    }

    return res;
  }
}
```