# Notes on stupidity

A repository dedicated to documenting the smarter ways of doing things.

## Printing "A | B | C | D"

Before:

```JSX
const { libraries, supersets, bundlers } = this.props;

const librariesMessage = libraries.reduce((accumulator, currentValue) => {
  return accumulator + " | " + currentValue + " | ";
});
const supersetsMessage = supersets.reduce((accumulator, currentValue) => {
  return accumulator + " | " + currentValue + " | ";
});
const bundlersMessage = bundlers.reduce((accumulator, currentValue) => {
  return accumulator + " | " + currentValue + " | ";
});

const completeMessage = librariesMessage + supersetsMessage + bundlersMessage;
```

After:

```JSX
const { libraries, supersets, bundlers } = this.props;
const completeMessage = [...libraries, ...supersets, ...bundlers].join(" | ");
```

## Ternary operator

Searching room by ID. Listing all the rooms if there is no user input, showing the search result if otherwise:

```JSX
let listElements;
let filteredList = rooms.filter(room => room.id.indexOf(input) > -1);
if (filteredList) {
  listElements = filteredList.map((room, index) => (
    <li key={index}>{room.id}</li>
  ));
} else {
  listElements = rooms.map(room => (
    <li key={index}>{room.id}</li>
  ));
}
```

Smart:

```JSX
const filteredList = rooms.filter(room => room.id.indexOf(input) > -1);
const listElements = (filteredList ? filteredList : rooms).map(room => (
  <li key={room.id}>{room.id}</li>
));
```

## De Morgan's Laws

Before:

```JavaScript
var _.some = function(collection, test) {
  var anyPass = false;

  for (var i = 0; i < collection.length; i++) {
    anyPass = anyPass || (test(collection[i]));
  }

  return anyPass;
};
```

After:

```JavaScript
var _.some = function(collection, test) {
  return !_.every(collection, function(element) {
    return !test(element);
  });
};
```

Source: [De Morgan's Laws - Erik's Blog](https://erikmhsiao.github.io/de-morgans-laws/)

## Combining duplicated items in an array

Combining all the items that have the same `time` value into a singular item, and using the sum of all their `views` as the `views` value:

```JavaScript
// Input
const array = [
  { time: 5, views: 2 },
  { time: 3, views: 1 },
  { time: 4, views: 3 },
  { time: 5, views: 7 }
];

// Output
[
  { time: 5, views: 9 },
  { time: 3, views: 1 },
  { time: 4, views: 3 }
];
```

Dumb:

```JavaScript
const compareItemToArray = (argArray, argIndex) => {
  const argItem = argArray[argIndex];
  argArray.map((currentItem, currentIndex) => {
    if (currentItem.time === argItem.time && currentIndex !== argIndex) {
      argItem.views += currentItem.views;
      argArray.splice(currentIndex, 1);
    }
  });
  return argArray;
};

const combineDuplicatedTimes = argArray => {
  argArray.map((item, i) => {
    compareItemToArray(argArray, i);
  });
  return argArray;
};

console.log(combineDuplicatedTimes(array));
```

Smart:

```JavaScript
const combineDuplicatedTimes = input => {
  return input.reduce((output, item) => {
    const result = output.find(i => i.time === item.time);
    if (result) result.views += item.views;
    else output.push(item);
    return output;
  }, []);
};

console.log(combineDuplicatedTimes(array));
```

Alternative (without using `Array.prototype.find()`):

```JavaScript
const combineDuplicatedTimes = input => {
  const output = [];
  for (const item of input) {
    const index = output.map(i => i.time).indexOf(item.time);
    if (index === -1) output.push(item);
    else output[index].views += item.views;
  }
  return output;
};

console.log(combineDuplicatedTimes(array));
```

Alternative (primitive):

```JavaScript
function indexOfbyKey(array, key, value) {
  for (var i = 0; i < array.length; i += 1) {
    if (array[i][key] === value) return i;
  }
  return -1;
}

function combineDuplicatedTimes(input) {
  var output = [];
  for (var i = 0; i < input.length; i += 1) {
    var index = indexOfbyKey(output, "time", input[i].time);
    if (index === -1) output.push(input[i]);
    else output[index].views += input[i].views;
  }
  return output;
}

console.log(combineDuplicatedTimes(array));
```

React will re-render if a state change is detected, thus `const` is okay. Do not use `index` as `key` for React component, `room.id` in this case will always be an unique string.

## Arrow function omits return

It could be more concise (and the missing return type made ESLint scream):

```TypeScript
export default function configureStore() {
  const store = createStore(
    playerReducer,
    composeWithDevTools()
  );

  return store;
}
```

Arrow function:

```TypeScript
const configureStore = (): Store => createStore(playerReducer, composeWithDevTools());
export default configureStore;
```

How I ended up doing:

```TypeScript
const store = createStore(rootReducer, composeWithDevTools());
export default store;
```

## PropTypes

One should also learn from others' mistake:

```JSX
class Breadcrumbs extends Component {
  const { links } = this.props;
}

Breadcrumbs.PropTypes = {
  links: PropTypes.array,
};
```

Error:

```Shell
'links' is missing in props validation react/prop-types
```

Solution is to change `Breadcrumbs.PropTypes` to `Breadcrumbs.propTypes`:

<a href="https://github.com/yannickcr/eslint-plugin-react/issues/1492"><img src="https://github.com/zw627/notes-on-stupidity/blob/master/img/prop-types.jpg" width="512px"></a>

## More
