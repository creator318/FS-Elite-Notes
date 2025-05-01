#Coding/DynamicProgramming 

You are managing a fleet of exploratory spacecraft, each carrying containers composed of two types of critical resources: fuel units (represented by '0') and oxygen tanks (represented by '1'). 

You're given a list 'containers', where each container is represented by a binary string indicating its resource composition, along with two integers: 'fuelLimit' (maximum allowed fuel units) and 'oxygenLimit' (maximum allowed oxygen tanks).

Your goal is to select the largest possible subset of containers such that the total number of fuel units does not exceed 'fuelLimit' and the total number of oxygen tanks does not exceed 'oxygenLimit'.

Input format:
-------------
Line 1: Space seperated strings, represents the container strings
Line 2: Two space separated integers, represents fuelLimit & oxygenLimit

Output format:
--------------
An integer, largest possible subset of containers.

Sample Input-1:
----------
```
10 0001 111001 1 0
5 3
```

Sample Output-1:
----------
```
4
```

Explanation:
----------
The largest subset meeting the constraints is {"10", "0001", "1", "0"}.
- Total fuel units = 5 (within limit)
- Total oxygen tanks = 3 (within limit)
Container "111001" cannot be included as it exceeds the oxygen tank limit.


Sample Input-2:
----------
```
10 0 1
1 1
```

Sample Output-2:
----------
```
2
```

Explanation:
----------
The largest subset meeting the constraints is {"0", "1"}.
- Total fuel units = 1
- Total oxygen tanks = 1


Constraints:
----------
- 1 <= containers.length <= 600
- 1 <= containers\[i].length <= 100
- 'containers\[i]' consists only of digits '0' and '1'.
- 1 <= fuelLimit, oxygenLimit <= 100

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    String containers[] = sc.nextLine().split(" ");
    
    int fuel = sc.nextInt(), oxygen = sc.nextInt();
    
    System.out.println(maxContainers(containers, fuel, oxygen));
    
    sc.close();
  }
  
  private static int maxContainers(String[] containers, int fuel, int oxygen) {
    int counts[][] = new int[containers.length][2];
    int dp[][][] = new int[containers.length][fuel+1][oxygen+1];
    
    for (int i=0; i<containers.length; i++)
      for (char ch: containers[i].toCharArray())
        counts[i][ch-'0']++;

    return selectContainers(counts, fuel, oxygen, 0, dp);
  }
  
  private static int selectContainers(int[][] counts, int fuel, int oxygen, int pos, int dp[][][]) {
    if (pos==counts.length) return 0;
    
    if (dp[pos][fuel][oxygen]!=0) return dp[pos][fuel][oxygen];
    
    int p = 0;
    if (counts[pos][0]<=fuel && counts[pos][1]<=oxygen)
      p = 1 + selectContainers(counts, fuel-counts[pos][0], oxygen-counts[pos][1], pos+1, dp);
      
    int np = selectContainers(counts, fuel, oxygen, pos+1, dp);
    
    return dp[pos][fuel][oxygen] = Math.max(p, np);
  }
}
```