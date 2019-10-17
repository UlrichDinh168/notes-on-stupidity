# Stupidity

A repository dedicated to documenting the smarter ways of doing things.

## Printing "A | B | C | D"

Dumb (and not working):

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

## Arrow function omit return

You are using arrow functions everywhere in your project, then for some reasons, you decided to play dumb (and the missing return type made ESLint scream):

```TypeScript
export default function configureStore() {
  const store = createStore(
    playerReducer,
    composeWithDevTools()
  );

  return store;
}
```

More concise though readability is debatable:

```TypeScript
const configureStore = (): Store => createStore(playerReducer, composeWithDevTools());
export default configureStore;
```

## More
