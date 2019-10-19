In Svelte you can listen for changes on the component state, and update other variables.

For example if you have a `count` variable:

```html
<script>
let count = 0
</script>
```

and you update it by clicking a button:

```html
<script>
let count = 0

const incrementCount = () => {
	count = count + 1
}
</script>

{count} <button on:click={incrementCount}>+1</button>
```

You can listen for changes on `count` using the special syntax `$:` which defines a new block that Svelte will re-run when any variable referenced into it changes.

Here's an example:

```html
<script>
let count = 0

const incrementCount = () => {
	count = count + 1
}

$: console.log(`${count}`)
</script>

{count} <button on:click={incrementCount}>+1</button>
```

I used the block:

```js
$: console.log(`${count}`)
```

You can write more than one of them:

```html
<script>
$: console.log(`the count is ${count}`)
$: console.log(`double the count is ${count * 2}`)
</script>
```

And you can also add a block to group more than one statement:

```html
<script>
$: {
  console.log(`the count is ${count}`)
  console.log(`double the count is ${count * 2}`)
}
</script>
```

I used a console.log() call in there, but you can update other variables too:

```html
<script>
let count = 0
let double = 0

$: {
  console.log(`the count is ${count}`)
  double = count * 2
  console.log(`double the count is ${double}`)
}
</script>
```