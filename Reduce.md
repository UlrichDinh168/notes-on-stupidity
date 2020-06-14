# Examples with reduce

When the function get called the first time, it will take the first parameter as its value in the array (can be modified),
the next value will be the next element in the array

```
const fruits = ['banana', 'coconut', 'strawberry', 'apple'];

const longestFruit = () => {
  return fruits.reduce(
 (longestFruitSoFar, currentFruit) => {
    if(longestFruitSoFar.length < currentFruit.length) {
      return currentFruit;
    }
    return longestFruitSoFar;
  })
};
// strawberry
```
However, in this example, when passing 'vesimeloni' into the initial value, it will replace 'banana' as its starter,
then proceed as normal.

``` const longestFruit = () => {
  return fruits.reduce(
 (longestFruitSoFar, currentFruit) => {
    if(longestFruitSoFar.length < currentFruit.length) {
      return currentFruit;
    }
    return longestFruitSoFar;
  },'vesimeloni')
};
// 'vesimeloni' places first but still lose to strawberry
```
The 'initial value', however could lead to a great thing (replacing Map, Filter or many combination of them)
```
const longestFruit = () => {
  return uppercaseFruits = fruits.reduce(
  (myNewUppercaseFruits, curFruit) => {
    return myNewUppercaseFruits.concat(curFruit +' '+ curFruit);
  },
  []
);
};
// return 2 fruits in one element
```



