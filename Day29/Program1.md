#Coding/FenwickTrees 

Imagine you are the chief curator at a futuristic museum. The museum consists of N exhibition halls arranged in a row, and each hall consists of certain number of rare artifacts. 
Your job is to keep track of the total number of artifacts on display and to manage special events that temporarily increase the artifact counts in a block of halls.

You need to support two types of operations:
1. Count Artifacts: Calculate the total number of artifacts displayed in the exhibition halls between indices start and end (inclusive).  
2. Host Special Exhibition: For a given range of halls (from start to end), increase the number of artifacts in each hall by a specified amount. 

This simulates a special exhibition event that attracts additional artifacts.

Input Format:
-------------
Line 1: Two integers N and Q, where N is the number of exhibition halls and Q is the number of operations.
Line 2: N space-separated integers representing the initial artifact counts in each hall.
Next Q Lines: Each line contains a query in one of the following formats:
  - For a Count Artifacts query:  
    1 start end
  - For a Host Special Exhibition event:  
    2 start end increment

Output Format:
--------------
For every Count Artifacts query (operation 1), output an integer representing the total number of artifacts in the specified range.

Sample Input-1:
-------
```
8 5
1 2 13 4 25 16 17 8
1 2 6
1 0 7
2 2 4 3
2 5 7 2
1 2 7
```

Sample Output-1:
-------
```
75
86
98
```


Explanation:
-------
- The initial artifact counts in the halls are: \[1, 2, 13, 4, 25, 16, 17, 8].
- Query 1: 1 2 6 → Sum halls 2 to 6: 13 + 4 + 25 + 16 + 17 = 75.
- Query 2: 1 0 7 → Sum halls 0 to 7: 1 + 2 + 13 + 4 + 25 + 16 + 17 + 8 = 86.
- Query 3: 2 2 4 3 → A special exhibition increases artifact counts in halls 2, 3, and 4 by 3. <br> New counts become: \[1, 2, 16, 7, 28, 16, 17, 8].
- Query 4: 2 5 7 2 → Another event boosts halls 5, 6, and 7 by 2.<br> New counts become: \[1, 2, 16, 7, 28, 18, 19, 10].
- Query 5: 1 2 7 → Sum halls 2 to 7: 16 + 7 + 28 + 18 + 19 + 10 = 98.


Constraints
-------
- 1 ≤ N ≤ $3 \cdot 10^4$
- -100 ≤ artifact count in each hall ≤ 100  
- 0 ≤ start ≤ end < N  
- -100 ≤ increment ≤ 100  
- At most $3 \cdot 10^4$ operations will be performed.

## Solution

```java
import java.util.*;

class FenwickTree {
  int sums[];

  FenwickTree(int[] arr) {
    sums = new int[arr.length+1];

    for (int i=0; i<arr.length; i++) add(i, arr[i]);
  }

  void add(int i, int val) {
    i++;

    while (i<=sums.length) {
      sums[i] += val;
      i += (i & -i);
    }
  }

  int prefixSum(int i) {
    i++;
    
    int sum = 0;
    while (i>0) {
      sum += sums[i];
      i -= (i & -i);
    }

    return sum;
  }

  int get(int i) {
    return prefixSum(i) - prefixSum(i-1);
  }

  int rangeSum(int l, int r) {
    return prefixSum(r) - prefixSum(l-1);
  }
}

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt(), q = sc.nextInt();
    
    int arr[] = new int[n];
    for (int i=0; i<n; i++) arr[i] = sc.nextInt();

    int queries[][] = new int[q][4];
    for (int i=0; i<q; i++) {
      int type = sc.nextInt();

      if (type==1) queries[i] =  new int[] {type, sc.nextInt(), sc.nextInt(), -1};
      else if (type==2) queries[i] = new int[] {type, sc.nextInt(), sc.nextInt(), sc.nextInt()};
    }

    System.out.println(countArtifacts(arr, queries));

    sc.close();
  }

  private static List<Integer> countArtifacts(int arr[], int queries[][]) {
    FenwickTree ft = new FenwickTree(arr);
    List<Integer> res = new LinkedList<>();

    for (int query[]: queries) {
      if (query[0]==1) 
        res.add(ft.rangeSum(query[1], query[2]));
      else if (query[0]==2)
        for (int i=query[1]; i<=query[2]; i++) {
          ft.add(i, query[3]);
        }
    }

    return res;
  }
}
```