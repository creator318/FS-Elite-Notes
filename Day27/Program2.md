#Coding/Trie 

Imagine you’re managing a busy cafe where every order is logged. You have a record—a list of dish names ordered throughout the day—and you want to determine which dishes are the most popular. Given an list of strings representing the dish names and an integer P, your task is to return the P most frequently ordered dishes.

The results must be sorted by the number of orders, from the most frequent to the least. If two dishes have been ordered the same number of times, they should be listed in alphabetical order.

Note: Use Tire data structure for this.

Input Format:
-------------
Line-1: comma separated line of words, list of words.
Line-2: An integer P, number of words to display. 

Output Format:
--------------
Print P number of most common used words.

Sample Input-1:
----------
```
coffee,sandwich,coffee,tea,sandwich,muffin
2
```

Sample Output-1:
----------
```
[coffee, sandwich]
```

Explanation:
----------
"coffee" and "sandwich" are the two most frequently ordered items. Although both appear frequently, "coffee" is placed before "sandwich" because it comes earlier alphabetically.

Sample Input-2:
----------
```
bagel,muffin,scone,bagel,bagel,scone,scone,muffin,muffin
3
```

Sample Output-2:
----------
```
[bagel, muffin, scone] 
```

Explanation: 
----------
"bagel", "muffin", and "scone" are the three most popular dishes with order frequencies of 3, 3, and 2 respectively. Since "bagel" and "muffin" have the same frequency, they are ordered alphabetically.

Constraints:
----------
- 1 <= orders.length <= 500  
- 1 <= orders\[i].length <= 10  
- Each orders\[i] consists of lowercase English letters.  
- P is in the range \[1, The number of unique dish names in orders].

## Solution:

```java
import java.util.*;

class Node {
  Node[] children;
  boolean end=false;
  int count=0;
  
  Node() {
    children = new Node[26];
  }
}

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner (System.in);
    
    String words[] = sc.nextLine().split(",");
    int n = sc.nextInt();

    System.out.println(mostFrequent(words, n));
    
    sc.close();
  }
  
  private static List<String> mostFrequent(String[] words, int n) {
    Node trie = new Node();

    for (String word: words) 
      insert(trie, word);

    Map<String, Integer> wordCounts = new HashMap<>();
    collect(trie, new StringBuilder(), wordCounts);

    List<Map.Entry<String, Integer>> wordCountsList = new LinkedList<>();
    wordCountsList.addAll(wordCounts.entrySet());

    Collections.sort(wordCountsList, 
      new Comparator<Map.Entry<String, Integer>>() {
        public int compare(Map.Entry<String, Integer> e1, Map.Entry<String, Integer> e2) {
          if (e1.getValue() != e2.getValue()) 
            return e2.getValue().compareTo(e1.getValue());
          else 
            return e1.getKey().compareTo(e2.getKey());
      }
    });
      
    List<String> res = new LinkedList<>();

    for (int i=0; i<n; i++) res.add(wordCountsList.get(i).getKey());

    return res;
  }
  
  private static void insert(Node node, String word) {
    for (char ch: word.toCharArray()) {
      int idx = ch-'a';
      
      if (node.children[idx]==null) node.children[idx] = new Node();
      node = node.children[idx];
    }
    
    node.end = true;
    node.count++;
  }
  
  private static void collect(Node node, StringBuilder curr, Map<String, Integer> words) {    
    if (node==null) return;

    if (node.end) words.put(curr.toString(), node.count);

    for (int i=0; i<26; i++) {
      if (node.children[i]!=null) {
        curr.append((char)('a'+i));
        collect(node.children[i], curr, words);
        curr.deleteCharAt(curr.length()-1);
      }
    }
  }
}
```