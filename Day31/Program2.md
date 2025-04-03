#Coding/SegmentTree

Coach Arjun is training a cricket team and is experimenting with a new fitness evaluation drill. He provided the fitness scores of N players in the team. As part of the drill, he asked the team manager to keep track and perform these two specific operations on the players' fitness scores:
1. bestFitness(start, end) - Report the maximum fitness score among the players whose jersey numbers lie between the positions start and end inclusive.
2. improveFitness(index, newScore) - Update the fitness score of the player at the specific index position with a new fitness score newScore.

Your task is to efficiently handle these requests by using a Segment Tree data structure.

Input Format:  
-------------
Line-1: Two integers N and Q, representing the number of players and the total 
        number of queries respectively.
Line-2: N space-separated integers representing the initial fitness scores of 
        the players.
The next Q lines: Each line contains three integers: 
- First integer (option) specifies the type of query:
- If option = 1, the next two integers (start, end) represent the range of jersey numbers (inclusive) for which to report the maximum fitness.
- If option = 2, the next two integers (index, newScore) represent the player's index to update and their new fitness score.

Output Format:  
--------------
An integer value for every bestFitness query, representing the maximum fitness score within the specified range.

Sample Input-1:  
-------------
```
8 5
1 2 13 4 25 16 17 28
1 2 6
1 0 7
2 2 18
2 4 36
1 2 7
```

Sample Output-1:  
--------------
```
25
28
36
```

## Solution:

```java
import java.util.*;

class SegmentTree {
  int[] tree;
  int n;

  SegmentTree(int[] arr) {
    n = arr.length;
    tree = new int[2 * n];
    System.arraycopy(arr, 0, tree, n, n);
   
     for (int i=n-1; i>0; i--) {
      tree[i] = Math.max(tree[i*2], tree[i*2 + 1]);
    }
  }

  int query(int l, int r) {
    int res = Integer.MIN_VALUE;
    
    for (l+=n, r+=n; l<=r; l/=2, r/=2) {
      if ((l&1)==1) res = Math.max(res, tree[l++]);
      if ((r&1)==0) res = Math.max(res, tree[r--]);
    }

    return res;
  }

  void update(int idx, int val) {
    for (tree[idx+=n]=val; idx>1; idx/=2)
      tree[idx/2] = Math.max(tree[idx], tree[idx^1]);
  }
}

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt(), q = sc.nextInt();

    int[] scores = new int[n];
    for (int i=0; i<n; i++) scores[i] = sc.nextInt();
    
    int queries[][] = new int[q][3];
    for (int i=0; i<q; i++) queries[i] = new int[] {sc.nextInt(), sc.nextInt(), sc.nextInt()};

    System.out.println(bestFitness(scores, queries));

    sc.close();
  }

  public static List<Integer> bestFitness(int[] scores, int[][] queries) {
    SegmentTree st = new SegmentTree(scores);
    List<Integer> res = new LinkedList<>();

    for (int[] query: queries) {
      if (query[0]==1) 
        res.add(st.query(query[1], query[2]));
      else if 
        (query[0]==2) st.update(query[1], query[2]);
    }

    return res;
  }
}
```