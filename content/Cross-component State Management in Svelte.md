We've already seen how Svelte makes handling the state of a single component very easy.

But how do we pass state around across components?

## Passing state around using props

The first strategy is common to other UI frameworks and it's passing state around using props, **lifting the state up**.

When a component needs to share data with another, the state can be moved up in the components tree until there's a common parent to those components.

The state needs to be passed down until it reaches all the components that need this state information.

This is done using **props**, and it's a technique that I think is the best as it's simple.

## The context API

However there are cases where props are not practical. Perhaps 2 components are so distant in the components tree that we'd have to move state up to the top level component.

In this case, another technique can be used and it's called **context API**, and it's ideal when you want to let multiple components communicate with descendants, but you don't want to pass props around.

The context API is provided by 2 functions which are provided by the `svelte` package: `getContext` and `setContext`.

You set an object in the context, associating it to a key:

```html
<script>
import { setContext } from 'svelte'

const someObject = {}

setContext('someKey', someObject)
</script>
```

Into another component you can use getContext to retrieve the object assigned to a key:

```html
<script>
import { getContext } from 'svelte'

const someObject = getContext('someKey')
</script>
```

You can only use `getContext` to retrieve a key either in the component that used `setContext`, or in one if its descendants.

If you want to let two components living in 2 different component trees communicate, there's another tool for us: **stores**.

## Using Svelte stores

Svelte stores are a great tool to handle your app state when components need to talk to each other without passing props around too much.

You must first import `writable` from `svelte/store`:

```js
import { writable } from 'svelte/store'
```

and create a store variable using the `writable()` function, passing the default value as the first argument:

```js
const username = writable('Guest')
```

This can be put into a separate file, which you can import into multiple components, for example called `store.js` (it's not a component, so it can be in a `.js` file instead of `.svelte`:

```js
import { writable } from 'svelte/store'
export const username = writable('Guest')
```

Any other component now loading this file can access the store:

```html
<script>
import { username } from './store.js'
</script>
```

Now the value of this variable can be set to a new value using `set()`, passing the new value as the first argument:

```js
username.set('new username')
```

And it can be updated using the `update()` function, which differs from `set()` because you don't just pass the new value to it - you run a callback function that is passed the current value as its argument:

```js
const newUsername = 'new username!'
username.update(existing => newUsername)
```

You can add more logic here:

```js
username.update(existing => {
  console.log(`Updating username from ${existing} to ${newUsername}`)
  return newUsername
})
```

To get the value of the store variable _once_, you can use the `get()` function exported by `svelte/store`:

```js
import { readable, get } from 'svelte/store'
export const username = writable('Guest')
get(username) //'Guest'
```

To create a reactive variable instead, that's updated whenever it changes, you can prepend the store variable using `$`, in this example `$username`. Using that will make the component rerender whenever the stored value changes.

> Svelte considers `$` to be a reserved value and will prevent you to use it for things that are not related to stores values (and which might lead to confusion), so if you are used to prepend DOM references using `$`, don't do it in Svelte.
>
Another option, best suited if you need to execute some logic when the variable changes, is to use the `subscribe()` method of `username`:

```js
username.subscribe(newValue => {
  console.log(newValue)
})
```

In addition to writable stores, Svelte provides 2 special kinds of stores: **readable stores** and **derived stores**.

### Svelte Readable Stores

Readable stores are special because they can't be updated from the outside - there's no `set()` or `update()` method. Instead, once you set the initial state, they can't be modified from the outside.

The official Svelte docs show an interesting example using a timer to update a date. I can think of setting up a timer to fetch resource from the network, perform an API call, get data from the filesystem (using a local Node.js server) or anything else that can be set up autonomously.

In this case instead of using `writable()` to initialize the store variable, we use `readable()`:

```js
import { readable } from 'svelte/store'
export const count = readable(0)
```

You can provide a function after the default value, that will be responsible for updating it. This function receives the `set` function to modify the value:

```html
<script>
import { readable } from 'svelte/store'
export const count = readable(0, set => {
  setTimeout(() => {
    set(1)
  }, 1000)
})
</script>
```

In this case, we update the value from 0 to 1 after 1 second.

You can setup an interval in this function, too:

```js
import { readable, get } from 'svelte/store'
export const count = readable(0, set => {
  setInterval(() => {
    set(get(count) + 1)
  }, 1000)
})
```

You can use this in another component like this:

```html
<script>
import { count } from './store.js'
</script>

{$count}
```

### Svelte Derived Stores

A derived store allows you to create a new store value that depends on the value of an existing store.

You can do so using the `derived()` function exported by `svelte/store`, which takes as first parameter the existing store value, and as a second parameter a function, which receives that store value as its first parameter:

```js
import { writable, derived } from 'svelte/store'

export const username = writable('Guest')

export const welcomeMessage = derived(username, $username => {
  return `Welcome ${$username}`
})
```

```html
<script>
import { username, welcomeMessage } from './store.js'
</script>

{$username}
{$welcomeMessage}
```
