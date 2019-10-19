Promises are an awesome tool we have at our disposal to work with asynchronous events in JavaScript.

The relatively recent introduction of the `await` syntax in ES2017 made using promises even simpler.

Svelte provides us the `{#await}` syntax in templates to directly work with promises at the template level.

We can wait for promises to resolve, and define a different UI for the various states of a promise: unresolved, resolved and rejected.

Here's how it works. We define a promise, and using the `{#await}` block we wait for it to resolve.

Once the promise resolves, the result is passed to the `{:then}` block:

```html
<script>
	const fetchImage = (async () => {
		const response = await fetch('https://dog.ceo/api/breeds/image/random')
    return await response.json()
	})()
</script>

{#await fetchImage}
	<p>...waiting</p>
{:then data}
	<img src={data.message} alt="Dog image" />
{/await}
```

You can detect a promise rejection by adding a `{:catch}` block:

```html
{#await fetchImage}
	<p>...waiting</p>
{:then data}
	<img src={data.message} alt="Dog image" />
{:catch error}
	<p>An error occurred!</p>
{/await}
```

Run the example: [https://svelte.dev/repl/70e61d6cc91345cdaca2db9b7077a941](https://svelte.dev/repl/70e61d6cc91345cdaca2db9b7077a941)