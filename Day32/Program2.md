#Coding/SegmentTree 

In Hyderabad after a long pandemic gap, the Telangana Youth festival is Organized at HITEX. In HITEX, there are a lot of programs planned. During the festival in order to maintain the rules of Pandemic, they put a constraint that one person can only attend any one of the programs in one day according to planned days.

Now it’s your aim to implement the "Solution" class in such a way that you need to return the maximum number of programs you can attend according to given constraints.

Explanation:
------------
You have a list of programs 'p' and days 'd', where you can attend only one program on one day. Programs \[p] = \[first day, last day], p is the program's first day and the last day.


Input Format:
-------------
Line-1: An integer N, number of programs.
Line-2: N comma separated pairs, each pair(f_day, l_day) is separated by space.

Output Format:
--------------
An integer, the maximum number of programs you can attend.


Sample Input-1:
---------------
```
4
1 2, 2 4, 2 3, 2 2
```

Sample Output-1:
----------------
```
4
```

Sample Input-2:
---------------
```
6
1 5, 2 3, 2 4, 2 2, 3 4, 3 5
```

Sample Output-2:
----------------
```
5
```

## Solution:

```java
import java.util.*;

class SegmentTree {
  int[] tree;
  int n;

  SegmentTree(int size) {
    n = size;
    tree = new int[2*n];
  }
  
  int get(int i) {
    return tree[n+i];
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
    
    int n = sc.nextInt();
    
    sc.nextLine();
    int[][] programs = new int[n][2];
    String[] input = sc.nextLine().split(",");
    for (int i=0; i<n; i++)
      programs[i] = Arrays.stream(input[i].trim().split(" ")).mapToInt(Integer::parseInt).toArray();
    
    System.out.println(maxAttendable(programs));
    
    sc.close();
  }
  
  public static int maxAttendable(int[][] programs) {
    Arrays.sort(programs, Comparator.comparingInt(a -> a[1]));
  
    int maxDay = programs[programs.length-1][1], res = 0;
  
    SegmentTree st = new SegmentTree(maxDay+1);

    for (int[] p: programs)
      if (st.query(p[0], p[1])<p[1]-p[0]+1)     
        for (int i=p[0]; i<=p[1]; i++)
          if (st.get(i)==0) {
            st.update(i, 1);
            res++;
            break;
          }
  
    return res;
  }
}
```