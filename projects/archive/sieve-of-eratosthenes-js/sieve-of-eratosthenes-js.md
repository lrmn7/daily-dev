**Last updated**: November 4, 2019 | at  04:10 AM



## SNIPPETS CODE

```js
const generatePrime = max => {
  let sieve = [2,3,5,7,11,13,17,19,23,29,31,37];
  const isPrimeFromSieve = num => {
    var max = Math.ceil(Math.sqrt(num));
    for (let i = 0; i < sieve.length; i++) {
      if (num % sieve[i] === 0) return false;
      else if (max < sieve[i]) return true;
    }
    return true;
  }
  let current = sieve[sieve.length - 1] + 2;
  while (current <= max) {
    if (isPrimeFromSieve(current)) sieve.push(current);
    current += 2;
  }
  return sieve
}

//EXAMPLE
//generate prime number from 1 to 100
console.log(generatePrime(100))
```