## Listening to DOM events

In Svelte you can define a listener for a DOM event directly in the template, using the `on:<event>` syntax.

For example to listen on the `click` event, you will pass a function to the `on:click` attribute.

To listen on the `onmousemove` event, you'll pass a function to the `on:mousemove` attribute.

Here's an example with the handling function defined inline:

```js
<button on:click={() => {
  alert('clicked')
}}>Click me</button>
```

and here's another example with the handling function defined in the `script` section of the component:

```js
<script>
const doSomething = () => {
  alert('clicked')
}
</script>

<button on:click={doSomething}>Click me</button>
```

I prefer inline when the code is not too verbose. If it's just 2-3 lines, for example, otherwise I'd move that up in the script section.

Svelte passes the event handler as the argument of the function, which is handy if you need to stop propagation or to reference something in the [Event object](https://flaviocopes.com/javascript-events/#the-event-object):

```js
<script>
const doSomething = event => {
  console.log(event)
  alert('clicked')
}
</script>

<button on:click={doSomething}>Click me</button>
```

Now, I mentioned "stop propagation". That's a very common thing to do, to stop form submit events for example. Svelte provides us **modifiers**, a way to apply it directly without manually doing it.
`stopPropagation` and `preventDefault` are the 2 modifiers you'll use the most, I think.

You apply a modifier like this: `<button on:click|stopPropagation|preventDefault={doSomething}>Click me</button>`

There are other modifiers, which are more niche. `capture` enables [capturing events instead of bubbling](https://flaviocopes.com/javascript-events/#event-bubbling-and-event-capturing), `once` only fires the event once, `self` only fires the event if the target of the event is this object (removing it from the bubbling/capturing hierarchy).

## Creating your own events in components

What's interesting is that we can create custom events in components, and use the same exact syntax of built-in DOM events.

To do so, we must import the `createEventDispatcher` function from the `svelte` package and call it to get an event dispatcher:

```html
<script>
  import { createEventDispatcher } from 'svelte'
  const dispatch = createEventDispatcher()
</script>
```

Once we do so, we can call the `dispatch()` function, passing a string that identifies the event (which we'll use for the `on:` syntax in other components that use this):

```html
<script>
  import { createEventDispatcher } from 'svelte'
  const dispatch = createEventDispatcher()

  //when it's time to trigger the event
  dispatch('eventName')
</script>
```

Now other components can use ours using

```html
<ComponentName on:eventName={event => {
  //do something
}} />
```

You can also pass an object to the event, passing a second parameter to `dispatch()`:

```html
<script>
  import { createEventDispatcher } from 'svelte'
  const dispatch = createEventDispatcher()
  const value = 'something'

  //when it's time to trigger the event
  dispatch('eventName', value)

  //or

  dispatch('eventName', {
    someProperty: value
  })
</script>
```

the object passed by `dispatch()` is available on the `event` object.
