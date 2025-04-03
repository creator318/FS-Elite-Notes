#JS/DeepCopy #Coding/Memoization

Implement a JavaScript ES6 module named AdvancedFactorialTool that calculates factorials of numbers provided by the user. The implementation should integrate the following ES6 features:

1. Implement your own ES6 class named CustomMap to mimic JavaScript's built-in Map.
2. Use the above-created CustomMap explicitly for caching factorial results to avoid redundant calculations.
3. Provide a deepClone function to clone the cache object deeply, protecting the internal state from accidental external modifications.
4. Implement a generator named factorialGenerator(n) which yields factorial results sequentially from 1! to n!.

## Solution

```js
class CustomMap {
  constructor(map) {
    this.items = {};
    this.count = 0;
  }
  
  set(key, val) {
    this.items[String(key)] = val;
    return val;
  }
  
  get(key) {
    return this.items[String(key)];
  }
  
  has(key) {
    return this.items.hasOwnProperty(String(key));
  }
}


const AdvancedFactorialTool = {
  cache: new CustomMap(),
  
  factorialMemoized(n) {
    if (n<=1) return this.cache.set(n, 1);
    if (this.cache.has(n)) return this.cache.get(n);
    
    this.cache.set(n, n*this.factorialMemoized(n-1));
    return this.cache.get(n);
  },

  deepClone(obj) {
    if (obj==null || typeof obj !== "object") return obj;
    if (Array.isArray(obj)) return obj.map(this.deepClone);
    
    const res = new CustomMap();
    for (const key in obj.items) 
      res.set(key, this.deepClone(obj[key]));
    
    return res;
  },

  *factorialGenerator(n) {
    let a=0, f=1;
    while ((a++)<n) yield f *= a;
  }
};

//Testing code
function testFactorialTool(n) {
  console.log(`Factorial of ${n} (Memoized):`, AdvancedFactorialTool.factorialMemoized(n));

  const originalCache = AdvancedFactorialTool.cache;
  const clonedCache = AdvancedFactorialTool.deepClone(originalCache);
  clonedCache.set(n, "modified");
  console.log("Original cache after clone modification:", originalCache.get(n));

  console.log(`Factorial sequence up to ${n}:`, [...AdvancedFactorialTool.factorialGenerator(n)]);
}

const readline = require('readline').createInterface({
  input: process.stdin,
  output: process.stdout
});

readline.question('Enter a positive integer to calculate factorial: ', input => {
  const n = parseInt(input.trim());

  if (isNaN(n) || n <= 0) {
    console.log('Please enter a valid positive integer.');
  } else {
    testFactorialTool(n);
  }

  readline.close();
});

```