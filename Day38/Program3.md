#Coding/DynamicProgramming 

You are a robot explorer navigating a vast digital maze stored as a string of digits. Each digit or pair of digits on the path represents a movement instruction, which translates to a specific direction using the following map:

"1" -> Move 'A'
"2" -> Move 'B'
...
"26" -> Move 'Z'

However, the maze has tricky encoding rules. Sometimes a single digit can be read alone, and sometimes two digits together form a valid move — but not every combination is valid. For example, "05" is invalid, while "5" is fine. Your robot can only navigate using valid instruction steps, and you must find how many unique navigation sequences the robot can follow to reach the end of the maze.

Given the string s of digits, determine the total number of distinct ways the robot can interpret and follow the path from start to end without making an invalid move.

If no valid navigation is possible, return 0.

Input Format:
-------------
A string s.

Output Format:
--------------
Print an integer result.

Sample Input-1:
---------------
```
123
```

Sample Output-1:
----------------
```
3
```

Explanation:
------------
123 can be converted as: ABC, LC, AW


Sample Input-2:
---------------
```
326
```

Sample Output-2:
----------------
```
2
```

Explanation:
------------
326 can be converted as: CBF, CZ

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    String s = sc.next();
    
    System.out.println(validPaths(s));
    
    sc.close();
  }
  
  private static int validPaths(String s) {
    int dp[] = new int[s.length()];
    Arrays.fill(dp, -1);
    
    return countPaths(s, 0, dp);
  }
  
  private static int countPaths(String s, int pos, int[] dp) {
    if (pos>=s.length()) return 1;
    if (s.charAt(pos)=='0') return 0;
    
    if (dp[pos]!=-1) return dp[pos];
    
    dp[pos] = countPaths(s, pos+1, dp);
    if (pos<s.length()-1) {
      int num = Integer.parseInt(s.substring(pos, pos+2));
      if (num>=10 && num<=26) dp[pos] += countPaths(s, pos+2, dp);
    }
    
    return dp[pos];
  }
}
```