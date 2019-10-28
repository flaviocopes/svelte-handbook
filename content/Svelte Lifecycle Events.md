Every component in Svelte fires several lifecycle events that we can hook into that help us implement the functionality we have in mind.

In particular, we have

- `onMount` fired after the component is rendered
- `onDestroy` fired after the component is destroyed
- `beforeUpdate` fired before the DOM is updated
- `afterUpdate` fired after the DOM is updated

We can schedule functions to happen when these events are fired by Svelte.

We don't have access to any of those methods by default, but we can import them from the `svelte` package:

```html
<script>
  import { onMount, onDestroy, beforeUpdate, afterUpdate } from 'svelte'
</script>
```

A common scenario for `onMount` is to fetch data from other sources.

Here's a sample usage of `onMount`:

```html
<script>
  import { onMount } from 'svelte'

  onMount(async () => {
    //do something on mount
  })
</script>
```

`onDestroy` allows us to clean up data or stop any operation we might have started at the component initialization, like timers or scheduled periodic functions using `setInterval`.

One particular thing to notice is that if we return a function from `onMount`, that serves the same functionality of `onDestroy` - it's run when the component is destroyed:

```html
<script>
  import { onMount } from 'svelte'

  onMount(async () => {
    //do something on mount

    return () => {
      //do something on destroy
    }
  })
</script>
```

Here's a practical example that sets a periodic function to run on mount, and removes it on destroy:

```html
<script>
  import { onMount } from 'svelte'

  onMount(async () => {
    const interval = setInterval(() => {
      console.log('hey, just checking!')
    }, 1000)

    return () => {
      clearInterval(interval)
    }
  })
</script>
```
