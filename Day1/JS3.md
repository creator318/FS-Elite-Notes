#JS  #JS/MapFilterReduce 

Given an array of product objects, each with name, price, 
And inStock (boolean). Use filter to include only products 
That are in stock, map to get their quantities, and reduce 
To find the total quantity of these in-stock products.


Sample Input:
-------------
```
5
Laptop 1000 5 true
Phone 500 3 false
Tablet 300 8 true
Monitor 200 6 false
Keyboard 50 12 true
```

Sample Output: 
--------------
```
25
```

Explanation:
------------
In-stock products are Laptop, Tablet, and Keyboard.
Quantities are 5, 8, and 12.
Total quantity = 5 + 8 + 12 = 25

## Solution:

```js
const readline = require("readline").createInterface({
  input: process.stdin,
  output: process.stdout
});

function solution(products) {
    return products
        .filter(p => p.inStock)
        .map(p => p.quantity)
        .reduce((a,n) => a+n, 0);
}

readline.question("", (N) => {
  N = parseInt(N);
  let products = [];
  let count = 0;

  readline.on("line", (line) => {
    const [name, price, quantity, inStock] = line.split(" ");
    products.push({ name, price: parseFloat(price), quantity: parseInt(quantity), inStock: inStock === 'true' });
    count++;

    if (count === N) {
      const totalQuantity = solution(products);
      console.log(totalQuantity);
      readline.close();
    }
  });
});
```