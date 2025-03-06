#Coding/DisjointSetUnion 

You are a database integrity engineer working for a global cloud company. 
Your team maintains a distributed database network, where each server either:
    - Stores equivalent data to another server (serverX == serverY).
    - Stores different data from another server (serverX != serverY).

The transitive consistency rule must be followed:
    - If A == B and B == C, then A == C must be true.
    - If A == B and B != C, then A != C must be true.

Your task is to analyze the given constraints and determine whether they 
follow transitive consistency. If all relations are consistent, return true; 
otherwise, return false

Input Format:
-------------
Space separated strnigs, list of relations

Output Format:
--------------
Print a boolean value, whether transitive law is obeyed or not.

Sample Input-1:
---------------
```
a==b c==d c!=e e==f
```

Sample Output-1:
----------------
```
true
```


Sample Input-2:
---------------
```
a==b b!=c c==a
```

Sample Output-2:
----------------
```
false
```

Explanation:
------------
{a, b} form one equivalence group.
{c} is declared equal to {a} (c == a), which means {a, b, c} should be equivalent.
However, b != c contradicts b == a and c == a.

Sample Input-3:
---------------
```
a==b b==c c!=d d!=e f==g g!=d
```

Sample Output-3:
----------------
```
true
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
  
    String[] s = sc.nextLine().split(" ");
    
    System.out.println(isTransitivityValid(s));
    sc.close();
  }

  public static boolean isTransitivityValid(String[] input) {
    Map<Character, Character> parent = new HashMap<>();
    
    ArrayList<String> notEqual = new ArrayList<>();
    for (String s : input) {
      if (s.charAt(1) != '!') union(s.charAt(0), s.charAt(3), parent);
      else notEqual.add(s);
    }

    for (String s : notEqual) {
      int p1 = find(s.charAt(0), parent), p2 = find(s.charAt(3), parent);
      if (p1==p2) return false;
    }

    return true;
  }

public static char find(char ch, Map<Character, Character> parent) {
  if (parent.get(ch)!=ch) parent.put(ch, find(parent.get(ch), parent));
  return parent.get(ch);
}

  public static void union(char ch1, char ch2, Map<Character, Character> parent) {
    char rootCh1 = find(ch1, parent), rootCh2 = find(ch2, parent);
    if (rootCh1 == rootCh2) return;

    if (rootCh1 > rootCh2) parent.put(rootCh2, rootCh1);
    else if (rootCh2 > rootCh1) parent.put(rootCh1, rootCh2);
  }  
}
```