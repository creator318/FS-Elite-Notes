#JS/MapFilterReduce 

You have an array of product objects, each with name, price, 
And sold (number of units sold). Use map to calculate the 
Revenue for each product, filter to include only products 
With a revenue of $50 or more, and reduce to find the total 
Revenue from these high-selling products.

Sample Input: 
-------------
```
5
Shampoo 5 15
Soap 2 20
Toothpaste 3 10
Conditioner 10 2
Lotion 8 10
```

Sample Output: 
--------------
```
155
```

Explanation:
------------
Shampoo revenue = 5 * 15 = 75
Soap revenue = 2 * 20 = 40 (not included, as it’s less than 50)
Toothpaste revenue = 3 * 10 = 30 (not included)
Conditioner revenue = 10 * 2 = 20 (not included)
Lotion revenue = 8 * 10 = 80


## Solution:

```js
const readline = require ("readline"). CreateInterface ({
  Input: process. Stdin,
  Output: process. Stdout
});

Function solution (products) {
  return products
    .map(p => p.price * p.sold)
    .filter(p => p >= 50)
    .reduce((a, n) => a+n, 0);  
}

Readline.Question ("", (N) => {
  N = parseInt (N);
  Let products = [];
  Let count = 0;

  Readline.On ("line", (line) => {
    Const [name, price, sold] = line.Split (" ");
    Products.Push ({ name, price: parseFloat (price), sold: parseInt (sold) });
    Count++;

    if (count === N) {
      const totalRevenue = solution(products);
      console.log(totalRevenue);
      readline.close();
    }
  });
});
```
