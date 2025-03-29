#Coding/FenwickTrees

Imagine you're the chief engineer aboard a futuristic spaceship. The ship is powered by N series of fuel cells arranged in a row, where each fuel cell holds a specific amount of energy measured in megajoules. Your job is to manage these fuel cells by performing two main operations:

- Option 1: Calculate the total energy available in a consecutive block of fuel cells between indices start and end (inclusive).
- Option 2: Update the energy level with given 'newEnergy' of a specific 'index' fuel cell when it's refilled.

Input Format:
-------------
Line-1: Two integers N and Q, where N is the number of fuel cells and Q is the number of operations.
Line-2: N space separated integers.
next Q lines: Three integers option, start index and end index/newEnergy.

Output Format:
--------------
An integer result, for every totalEnergy.


Sample Input-1:
-----------
```
8 5
1 2 13 4 25 16 17 8
1 2 6 
1 0 7 
2 2 18
2 4 17
1 2 7 
```

Sample Output-1:
----------
```
75
86
80
```

Sample Input-2:
----------
```
16 1
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16
1 0 15
```

Sample Output-2:
----------
```
136
```


Constraints:
----------
- 1 <= N <= $3 \cdot 10^4$  
- -100 <= fuelCells\[i] <= 100  
- 0 <= index < fuelCells.length  
- -100 <= newEnergy <= 100  
- 0 <= start <= end < fuelCells.length  
- At most $3 \cdot 10^4$ calls will be made to recharge and totalEnergy.

## Solution:

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

    int queries[][] = new int[q][3];
    for (int i=0; i<q; i++) queries[i] = new int[] {sc.nextInt(), sc.nextInt(), sc.nextInt()};
    
    System.out.println(findEnergies(arr, queries));

    sc.close();
  }

  private static List<Integer> findEnergies(int arr[], int queries[][]) {
    FenwickTree ft = new FenwickTree(arr);
    List<Integer> res = new LinkedList<>();

    for (int query[]: queries) {
      if (query[0]==1) 
        res.add(ft.rangeSum(query[1], query[2]));
      else if (query[0]==2)
        ft.add(query[1], query[2]-ft.get(query[1]));
    }

    return res;
  }
}
```