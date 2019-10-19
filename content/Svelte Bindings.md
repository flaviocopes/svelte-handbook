Using Svelte you can create a two-way binding between data and the UI.

Many other Web frameworks can provide two-way bindings, it's a very common pattern.

They are especially useful with forms.

## bind:value

Let's start with most common form of binding you'll often use, which you can apply using `bind:value`. You take a variable from the component state, and you bind it to a form field:

```html
<script>
let name = ''
</script>

<input bind:value={name}>
```

Now if `name` changes the input field will update its value. And the opposite is true, as well: if the form is updated by the user, the `name` variable value changes.

> Just be aware that the variable must be defined using `let/var` and not `const`, otherwise it can't be updated by Svelte, as `const` defines a variable with a value that can't be reassigned.

`bind:value` works on all flavors of input fields (`type="number"`, `type="email"` and so on), but it also works for other kind of fields, like `textarea` and `select` (more on `select` later).

## Checkboxes and radio buttons

Checkboxes and radio inputs (`input` elements with `type="checkbox"` or `type="radio"`) allow those 3 bindings:

- `bind:checked`
- `bind:group`
- `bind:indeterminate`

`bind:checked` allows us to bind a value to the checked state of the element:

```html
<script>
let isChecked
</script>

<input type=checkbox bind:checked={isChecked}>
```

 `bind:group` is really handy with checkboxes and radio inputs, because those are very often used in groups. Using `bind:group` you can associate a JavaScript array to a list of checkboxes, and have it populated based on the choices made by the user.

Here's an example. The `goodDogs` array populates based on the checkboxes I tick:

```html
<script>
let goodDogs = []
let dogs = ['Roger', 'Syd']
</script>

<h2>
	Who's a good dog?
</h2>

<ul>
  {#each dogs as dog}
	  <li>{dog} <input type=checkbox bind:group={goodDogs} value={dog}></li>
  {/each}
</ul>

<h2>
	Good dogs according to me:
</h2>

<ul>
  {#each goodDogs as dog}
	  <li>{dog}</li>
  {/each}
</ul>
```

See the example on [https://svelte.dev/repl/059c1b5edffc4b058ad36301dd7a1a58](https://svelte.dev/repl/059c1b5edffc4b058ad36301dd7a1a58)

`bind:indeterminate` allows us to bind to the `indeterminate` state of an element (if you want to learn more head to [https://css-tricks.com/indeterminate-checkboxes/](https://css-tricks.com/indeterminate-checkboxes/))

## Select fields

`bind:value` also works for the `select` form field to get the selected value automatically assigned to the value of a variable:

```html
<script>
let selected
</script>

<select bind:value={selected}>
  <option value="1">1</option>
  <option value="2">2</option>
  <option value="3">3</option>
</select>

{selected}
```

The cool thing is that if you generate options dynamically from an array of objects, the selected option is now an object, not a string:

```html
<script>
let selected

const goodDogs = [
	{ name: 'Roger' },
	{ name: 'Syd' }
]
</script>

<h2>List of possible good dogs:</h2>
<select bind:value={selected}>
	{#each goodDogs as goodDog}
    <option value={goodDog}>{goodDog.name}</option>
	{/each}
</select>

{#if selected}
<h2>
	Good dog selected: {selected.name}
</h2>
{/if}
```

See example: [https://svelte.dev/repl/7e06f9b7becd4c57880db5ed184ea0f3](https://svelte.dev/repl/7e06f9b7becd4c57880db5ed184ea0f3)

`select` also allows the `multiple` attribute:

```html
<script>
let selected = []

const goodDogs = [
	{ name: 'Roger' },
	{ name: 'Syd' }
]
</script>

<h2>List of possible good dogs:</h2>
<select multiple bind:value={selected}>
	{#each goodDogs as goodDog}
    <option value={goodDog}>{goodDog.name}</option>
	{/each}
</select>

{#if selected.length}
<h2>Good dog selected:</h2>
<ul>
	{#each selected as dog}
	  <li>{dog.name}</li>
	{/each}
</ul>
{/if}
```

See example:  [https://svelte.dev/repl/b003248e87f04919a2f9fed63dbdab8c](https://svelte.dev/repl/b003248e87f04919a2f9fed63dbdab8c)

## Other bindings

Depending on the HTML tag you are working on, you can apply different kind of bindings.

`bind:files` is a binding valid on `type="file"` input elements, to bind the list of selected files.

The `details` HTML element allows the use of `bind:open` to bind its open/close value.

The `audio` and `video` media HTML tags allow to bind several of their properties: `currentTime`, `duration`, `paused`, `buffered`, `seekable`, `played`, `volume`, `playbackRate`.

`textContent` and `innerHTML` can be bound on `contenteditable` fields.

All things very useful for those specific HTML elements.

## Read only bindings

`offsetWidth`, `offsetHeight`, `clientWidth`, `clientHeight` can be bound, read only, on any block level HTML element, excluding void tags (like `br`) and elements that are set to be inline (`display: inline`).

## Get a reference to the HTML element in JavaScript

`bind:this` is a special kind of binding that allows you to get a reference to an HTML element and bind it to a JavaScript variable:

```html
<script>
let myInputField
</script>

<input bind:this={myInputField} />
```

This is really handy when you need to apply logic to elements after you mount them, for example, using the `onMount()` lifecycle event callback.

## Binding components props

Using `bind:` you can bind a value to any prop that a component exposes.

Say you have a `Car.svelte` component:

```html
<script>
export let inMovement = false
</script>

<button on:click={() => inMovement = true }>Start car</button>
```

You can import the component and bind the `inMovement` prop:

```html
<script>
	import Car from './Car.svelte';

	let carInMovement;
</script>

<Car bind:inMovement={carInMovement} />

{carInMovement}
```

This can allow interesting scenarios.