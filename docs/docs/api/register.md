---
sidebar_position: 1
---

# register

The `register` function is used to wrap your component / screen into a separate bundle.

## Typescript Usage

You may optionally supply a generic type `P` to enforce typings of the returned wrapped object:

```
type MyProps = { hello: string }

const BundledComponent = register<MyProps>({ loader: () => import('path/to/component') })

<BundledComponent hello="world">
```

## Params

### `loader`

A function that returns a React Component. Example:

```
const BundledComponent = register({ loader: () => import('./Component') })
```

### `require`

> 🚨 Deprecated. Use `loader` instead.

A function that returns a React Element. Example:

```
const BundledComponent = register({ require: () => require('./Component.js') })
```

### `name`

**Optional.** A string that can be later used with `preload` to preload components before they're rendered.

### `group`

**Optional.** A string that can be later used with `preload` to preload groups of components before they're rendered.

### `static`

> Related documentation: [Static Options](../guides/static-options).

**Optional.** An object with members you'd like to statically access:

```
// Previously
const ListItem = () => {}
const List = () => {}

List.Item = ListItem // ❌ This reference becomes unreachable after the component is wrapped with `register()`

// Use `static` instead:
Const BundledComponent = register({
  loader: () => import('...'),
  static: {
    Item: ListItem
  }
})

BundledComponent.Item // ✅
```

### `cached`

**Optional.** Default: `true`.

Set to `false` if you don't need cache your components.

**Warning:** disabling caching may decrease performance of your application.

### `placeholder`

**Optional.** React Component to be displayed during the first render of a registered component. Rendering a placeholder provides a better visual experience for users if you're not `preload`ing registered component.

### `extract`

**Optional.** A string indicating the name of a named export. If your `loader` function is importing a file with one of more named exports, you may wish to provide the name of an individual component to be bundled:

```
// Component.js
export const Component1 = () => {}
export const Component2 = () => {}

// SomewhereElse.js
const BundledComponent = register({
  loader: () => import('./Component'),
  extract: 'Component1'
})
```
