#Coding  #Coding/SlidingWindow 

In a software development company, a team works on various projects over n weeks. 
The team completes a certain number of tasks tasks \[i] each week and dedicates 
Hours\[i] hours of work. Given an integer k, for every consecutive sequence of 
K weeks (tasks\[i], tasks\[i+1], ..., tasks\[i+k-1] and 
Hours\[i], hours\[i+1], ..., hours\[i+k-1] for all 0 <= i <= n-k), 
They evaluate T, the total number of tasks completed during that sequence 
Of k weeks, and E, the total hours of work during that sequence of k weeks:

a) If T < lower and E >= work_goal, the team performed very poorly and loses 2 points
B) If T < lower and E < work_goal, the team performed poorly and loses 1 point.
C) If T >= upper and E >= work_goal, the team performed well and gains 1 point.
D) If T >= upper and E < work_goal, the team performed exceptionally well and gains 2 points.
E) Otherwise, the team's performance is normal and there is no change in points.

Initially, the team starts with zero points. Return the total number of points 
The team has after working for n weeks. Note that the total points can be negative.

Sample Input-1:
-----------------
```
5 // n
10 20 30 40 50 // tasts
30 20 10 30 40 // hours
2 // k
35 // lower
70 // upper
45 // goal
```

Sample Output-1:
-----------------
```
1
```

Explanation:
-----------------
For \[10, 20] and \[30, 20]:
T = 30 < lower and E = 50 >= work_goal, the team performed very poorly and loses 2 points.

For \[20, 30] and \[20, 10]:
T = 50 >= lower and T <= upper and E = 30 < work_goal, no change in points.

For \[30, 40] and \[10, 30]:
T = 70 = upper and E = 40 < work_goal, the team performed exceptionally well and 
Gains 2 points.

For \[40, 50] and \[30, 40]:
T = 90 > upper and E = 70 >= work_goal, the team performed well and gains 1 point.

Therefore, the team gains 1 point (0 - 2 + 2 + 1 = 1).


Sample Input-2:
-----------------
```
4	// n
5 8 10 15 // tasks
25 30 20 25 // hours
3	// k
25 // lower
40 // upper
60 // goal
```

Sample Output-2:
-----------------
```
-2
```

## Solution: 

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int n = sc.nextInt();
        
        int tasks[] = new int[n];
        for (int i=0; i<n; i++) tasks[i] = sc.nextInt();
        
        int hours[] = new int[n];
        for (int i=0; i<n; i++) hours[i] = sc.nextInt();
        
        int k = sc.nextInt();
        int tl = sc.nextInt(), tu = sc.nextInt();
        int wg = sc.nextInt();
        
        System.out.println(slide(tasks, hours, k, tl, tu, wg));
        
        sc.close();
    }
    
    private static int slide(int tasks[], int hours[], int k, int tl, int tu, int wg) {
        int score = 0;
        int T=0, E=0;
        
        for (int i=0; i<k; i++) {
            T += tasks[i];
            E += hours[i];
        }
        
        for (int i=0; i<=tasks.length-k; i++) {
            if (T<tl && E>=wg) score -= 2;
            else if (T<tl && E<wg) score -= 1;
            else if (T>=tu && E>=wg) score += 1;
            else if (T>=tu && E<wg) score += 2;
            
            if (i==tasks.length-k) continue;
            T += tasks[i+k] - tasks[i];
            E += hours[i+k] - hours[i];
        }
        
        return score;
    }
}
```