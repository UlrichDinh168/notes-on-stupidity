# Examples with reduce
## Syntax
```
arr.reduce(callback( accumulator, currentValue[, index[, array]] )[, initialValue])
```
### Parameters
#### callback
A function to execute on each element in the array (except for the first, if no initialValue is supplied).
It takes four arguments:

```
 ACCUMULATOR : The accumulator accumulates callback's return values. It is the accumulated value previously returned in the last invocation of the callback—or initialValue, if it was supplied (see below).
 CURRETNVALUE : The current element being processed in the array.
 INDEX (Optional) : The index of the current element being processed in the array. Starts from index 0 if an initialValue is provided. Otherwise, it starts from index 1.
 ARRAY (Optional) : The array reduce() was called upon.
 INITIAL VALUE (Optional) : A value to use as the first argument to the first call of the callback. If no initialValue is supplied, the first element in the array will be used as the initial accumulator value and skipped as currentValue. Calling reduce() on an empty array without an initialValue will throw a TypeError.
```


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

Here is a another example

```
const cart = [
  { name: 'banana', quantity: 5 },
  { name: 'cheese', quantity: 2 },
  { name: 'bread', quantity: 1 },
]

const userCart = [
  { name: 'banana', quantity: 2 },
  { name: 'watermelon', quantity: 1 },
  { name: 'cucumber', quantity: 10 },
  { name: 'bread', quantity: 4 }
]

// updatedCart is the function that responsible for updating new items

//newCart's first value is cart
//curItem's first value is giang[0]
const updatedCart = userCart.reduce((newCart, curItem) => {
  //curItemIndex finds INDEX that matched names in cart and userCart
  //findIndex return the INDEX of first element that match the criteria
  const curItemIndex = newCart.findIndex(i => i.name === curItem.name)
  if (curItemIndex >= 0) { //if there is a match
    return [
      // ...newCart.slice(0, curItemIndex),
      // // {
      // //   name: curItem.name,
      // //   quantity: curItem.quantity + newCart[curItemIndex].quantity,
      // // },
      ...newCart.slice(curItemIndex + 1),
    ]
  }
  return [...newCart, curItem]
}, cart)

console.log(updatedCart)
```



