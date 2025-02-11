#Coding  #Coding/SlidingWindow 

In a film festival, there is a lineup of movies, each with a rating. The festival 
Organizer wants to find the maximum total rating of 'k' sequence of movies while 
Following these rules:
    1. The sequence must be exactly k movies long.
    2. Each movie in the sequence must have a distinct rating.
    3. None of the movies in the sequence should have a restricted rating, as 
       These are reserved for special screenings.

Given an array movieRatings representing the sequence of movie ratings, an integer k 
Representing the length of the sequence, and a set restrictedRatings (of size m) of 
Special ratings, find the maximum total rating of any valid sequence. 
If no valid sequence exists, return -1.

Input Format:
-------------
Line-1: 3 space separated integers, n, k, m
Line-2: n space separated integers, movieRatings[].
Line-3: m space separated integers, restrictedRatings[].

Output Format:
-------------
An integer, the maximum total rating of any valid sequence


Sample Input-1:
---------------
7 3 2
1 5 4 2 9 9 9
2 9

Sample Output-1:
----------------
10

Explanation: 
------------
The sequences of movie ratings with length 3 are:
- [1, 5, 4] which meets the requirements and has a total rating of 10.
- [5, 4, 2] which does not meet the requirements because 2 is in the 
Restricted set.
- [4, 2, 9] which does not meet the requirements because 2 and 9 are in 
The restricted set.
- [2, 9, 9] which does not meet the requirements because 2 and 9 are in 
The restricted set and 9 is repeated.
- [9, 9, 9] which does not meet the requirements because 9 is in 
The restricted set and repeated.

We return 10 because it is the maximum total rating of all the sequences 
That meet the conditions.


Sample Input-2:
---------------
3 3 1
4 4 4
4

Sample Output-2:
----------------
-1

Explanation: The sequences of movie ratings with length 3 are:
[4, 4, 4] which does not meet the requirements because 4 is repeated and in the restricted set.
We return -1 because no sequences meet the conditions.

Constraints:
------------
0 <= m <= n <=1000
K <= n
0 ≤ restrictedRatings. Length ≤ 1000

## Solution:

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int n = sc.nextInt(), k = sc.nextInt(), m = sc.nextInt();
        
        int ratings[] = new int[n];
        for (int i=0; i<n; i++) ratings[i] = sc.nextInt();
        
        
        Set<Integer> restricted = new HashSet<>();
        for (int i=0; i<m; i++) restricted.add(sc.nextInt());
        
        System.out.println(slide(ratings, restricted, k));
        
        sc.close();
    }
    
    private static int slide(int ratings[], Set<Integer> restricted, int k) {
        int res = Integer.MIN_VALUE;
        
        for (int i=0; i<=ratings.length-k; i++) {
            Set<Integer> s = new HashSet<>();
            for (int j=0; j<k; j++) 
                if (!restricted.contains(ratings[i+j]) && !s.contains(ratings[i+j]))
                    s.add(ratings[i+j]);
            
            if (s.size() == k) res = Math.max(res, s.stream().mapToInt(Integer::intValue).sum());
        }
        return res == Integer.MIN_VALUE ? -1 : res;
    }
}
```