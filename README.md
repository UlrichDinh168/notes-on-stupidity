# Stupidity

A repository dedicated to recording the smarter ways of doing things.

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

## More
