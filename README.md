# Stupidity

A repository dedicated to documenting the smarter ways of doing things.

## Printing "A | B | C | D"

Dumb and not working:

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

Smart:

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
