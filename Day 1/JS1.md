#JS/MapFilterReduce

You have an array of project objects, each with name, 
Revenue, and cost. Use filter to include only projects 
Where revenue is greater than cost (profitable projects), 
Map to calculate the profit for each project, and reduce 
To find the total profit from these profitable projects.

Sample Input:
-------------
5
ProjectA 500 300
ProjectB 200 250
ProjectC 600 400
ProjectD 150 100
ProjectE 300 400

Sample Output: 
--------------
450

Explanation:
------------
Profitable projects are ProjectA, ProjectC, and ProjectD.
Profits for each are: ProjectA = 200, ProjectC = 200, ProjectD = 50.
Total profit = 200 + 200 + 50 = 450

## Solution : 

```js
const readline = require ("readline"). CreateInterface ({
  Input: process. Stdin,
  Output: process. Stdout
});

Function solution (projects) {
    Return projects
        .filter (p => p.cost < p.revenue)
        .map (p => p.revenue-p.cost)
        .reduce ((a, n) => a+n, 0);
}

Readline.Question ("", (N) => {
  N = parseInt (N);
  Let projects = [];
  Let count = 0;

  Readline.On ("line", (line) => {
    Const [name, revenue, cost] = line.Split (" ");
    Projects.Push ({ name, revenue: parseFloat (revenue), cost: parseFloat (cost) });
    Count++;

    if (count === N) {
      const totalProfit = solution(projects);
      console.log(totalProfit);
      readline.close();
    }
  });
});
```