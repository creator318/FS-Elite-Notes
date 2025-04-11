#Coding/BackTracking 

Given a classic mobile phone, and the key pad of the phone looks like below.

| 1   | 2   | 3   |
| --- | --- | --- |
|     | abc | def |

| 4   | 5   | 6   |
| --- | --- | --- |
| ghi | jkl | mno |
  
| 7    | 8   | 9    |
| ---- | --- | ---- |
| pqrs | tuv | wxyz | 
	
| *   | 0   | #   |
| --- | --- | --- |


You are given a string S contains digits between \[2-9] only, For example: S = "2", then the possible words are "a", "b", "c".

Now your task is to find all possible words that the string S could represent and print them in a lexicographical order. 

Input Format:
-------------
A string S, consist of digits \[2-9]

Output Format:
--------------
Print the list of words in lexicographical order.

Sample Input-1:
---------------
```
2
```

Sample Output-1:
----------------
```
[a, b, c]
```


Sample Input-2:
---------------
```
24
```

Sample Output-2:
----------------
```
[ag, ah, ai, bg, bh, bi, cg, ch, ci]
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    String s = sc.next();
    
    System.out.println(getCombinations(s));
    
    sc.close();
  }
  
  public static List<String> getCombinations(String s) {
    HashMap<Character, List<Character>> keypad = new HashMap<>() {{
      put('2', Arrays.asList('a', 'b', 'c'));
      put('3', Arrays.asList('d', 'e', 'f'));
      put('4', Arrays.asList('g', 'h', 'i'));
      put('5', Arrays.asList('j', 'k', 'l'));
      put('6', Arrays.asList('m', 'n', 'o'));
      put('7', Arrays.asList('p', 'q', 'r', 's'));
      put('8', Arrays.asList('t', 'u', 'v'));
      put('9', Arrays.asList('w', 'x', 'y', 'z'));
    }};

    List<String> res = new LinkedList<>();
    StringBuilder sb = new StringBuilder();

    backtrack(sb, s, res, keypad, 0);

    return res;
  }

  public static void backtrack(StringBuilder sb, String s, List<String> res, Map<Character, List<Character>> keypad, int curr) {
    if (curr==s.length()) {
      res.add(sb.toString());
      return;
    }

    List<Character> letters = keypad.get(s.charAt(curr));
    
    for (int i=0; i<letters.size(); i++) {
      sb.append(letters.get(i));
      backtrack(sb, s, res, keypad, curr+1);
      sb.deleteCharAt(sb.length()-1);
    }
  }
}
```