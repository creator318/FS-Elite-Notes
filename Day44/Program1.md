#Coding/DynamicProgramming 

Archaeologists discovered an ancient manuscript represented by a string 'text', where each character represents an ancient symbol. They suspect the manuscript might contain repeated symbol patterns (substrings).

Your task is to analyze the text and determine the length of the longest repeating symbol pattern. If the text contains no repeating patterns, return '0'.

Sample Input-1:
--------
```
scarabankhscarab
```

Sample Output-1:
--------
```
6
```

Explanation:
--------
The longest repeating symbol pattern is "scarab", appearing twice.

Constraints:
--------
- 1 <= text. Length <= 2000
- 'text' consists of lowercase English letters ('a' - 'z').

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
  	Scanner sc = new Scanner(System.in);
  	
  	String s = sc.next();
  	
  	System.out.println(longestRepeating(s));
  	
  	sc.close();
  }
  
  private static int longestRepeating(String s) {
  	int n = s.length(), res = 0;
  	int dp[][] = new int[n+1][n+1];
  	
  	for (int i=0; i<n; i++) {
  		for (int j=0; j<n; j++) {
  			if (s.charAt(i)==s.charAt(j) && i!=j)
  				dp[i+1][j+1] = dp[i][j] + 1;
  			else
  				dp[i+1][j+1] = 0;
  				
				res = Math.max(res, dp[i+1][j+1]);
		  }
		}
		
		return res;
  }
}
```