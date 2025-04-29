#Coding/DynamicProgramming 

You are working in a genetics laboratory where you are tasked with correcting DNA sequences. Each DNA strand is represented as a sequence of characters 'A', 'C', 'G', and 'T'.
Sometimes, due to mutations or errors during sequencing, the DNA strand (originalDNA) must be modified to match a targetDNA sequence exactly.

You can perform the following mutation operations:
- Insert a nucleotide (A, C, G, or T) into the DNA strand.
- Delete a nucleotide from the DNA strand.
- Replace a nucleotide with another one.

Each operation counts as one step.

Your task is to find the minimum number of mutation operations needed to transform the originalDNA into the targetDNA.

Input format:
-------------
2 space seperated strings, originalDNA and targetDNA

Output format:
--------------
An integer, the minimum number of mutation operations


Sample Input-1:
-----------
```
ACGT AGT
```

Sample Output-1:
-----------
```
1
```

Explanation:
-----------
- Delete 'C': "ACGT" → "AGT"
Only 1 mutation is needed.

Sample Input-2:
-----------
```
GATTAC GCATGCU
```

Sample Output-2:
-----------
```
4
```

Explanation:
-----------
- Replace 'A' with 'C': "GATTAC" → "GCTTAC"
- Replace 'T' with 'A': "GCTTAC" → "GCATAC"
- Replace 'A' with 'G': "GCATAC" → "GCATGC"
- Insert 'U' at the end: "GCATGC" → "GCATGCU"

Thus, 4 mutations are needed.

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String args[]) {
    Scanner sc = new Scanner(System.in);
    
    String src = sc.next(), dst = sc.next();

    System.out.println(minMutations(src, dst));

    sc.close();
  }

  private static int minMutations(String src, String dst) {
    int dp[][] = new int[src.length() + 1][dst.length() + 1];

    return getMutations(src, dst, 0, 0, dp);
  }

  private static int getMutations(String src, String dst, int p1, int p2, int[][] dp) {
    if (p1==src.length() || p2==dst.length()) return Math.max(src.length()-p1, dst.length()-p2);

    if (dp[p1][p2]!=0) return dp[p1][p2];

    if (src.charAt(p1)==dst.charAt(p2))
      return dp[p1][p2] = getMutations(src, dst, p1+1, p2+1, dp);
    else {
      int min = Integer.MAX_VALUE;
      
      min = Math.min(min, getMutations(src, dst, p1+1, p2, dp));
      min = Math.min(min, getMutations(src, dst, p1, p2+1, dp));
      min = Math.min(min, getMutations(src, dst, p1+1, p2+1, dp));
      
      return dp[p1][p2] = min + 1;
    }
  }
}
```