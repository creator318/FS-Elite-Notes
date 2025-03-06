
"Emphatic Pronunciation" of a given word is where we take the word and replicate some of the letter to emphasize their impact.

Instead of saying 'oh my god', someone may say "ohhh myyy goddd", We define emphatic pronunciation of a word, which is derived by replicating a group (or single) of letters in the original word. 

So that the replicated group is atleast 3 characters or more and greater than or equal to size of original group. 
For example Good -> Goood is an emphatic pronunciation, but Goodd is not because in Goodd the 'd' are only occuring twice consecutively.
 
In the question you are given the "Emphatic pronunciation" word, you have to findout how many words can legal result in the "emphatic pronunciation" word.

Input Format:
-------------
Line-1 -> A String contains a single word, Emphatic Pronunciation word
Line-2 -> Space seperated word/s

Output Format:
--------------
Print an integer as your result


Sample Input-1:
---------------
```
goood
good goodd
```

Sample Output-1:
----------------
```
1
```


Sample Input-2:
---------------
```
heeelllooo
hello hi helo
```

Sample Output-2:
----------------
```
2
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String [] args){
    Scanner sc = new Scanner(System.in);

    String em = sc.nextLine();
    String words [] = sc.nextLine().split(" ");

    System.out.println(countMatching(em,words));

    sc.close();
  }

  static int countMatching(String em, String words[]){
    Map<Character,Integer> map = new HashMap<>();
    Map<Character,Integer> freq = new HashMap<>();

    for (char ch: em.toCharArray()) map.put(ch, map.getOrDefault(ch, 0)+1);

    int count = 0;
    outer : for (String word : words) {
      freq.clear();
      for (char ch : word.toCharArray())
        if (map.containsKey(ch)) {
          freq.put(ch, freq.getOrDefault(ch, 0)+1);
          if(freq.get(ch)>map.get(ch)) continue outer;
        } else continue outer;
      count++;
    }

    return count;
  }
}
```

