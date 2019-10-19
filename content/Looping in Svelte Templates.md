In Svelte templates you can create a loop using the `{#each}{/each}` syntax:

```html
<script>
let goodDogs = ['Roger', 'Syd']
</script>

{#each goodDogs as goodDog}
  <li>{goodDog}</li>
{/each}
```

If you are familiar with other frameworks that use templates, it's a very similar syntax.

You can get the index of the iteration using:

```html
<script>
let goodDogs = ['Roger', 'Syd']
</script>

{#each goodDogs as goodDog, index}
  <li>{index}: {goodDog}</li>
{/each}
```

(indexes start at 0)

When dynamically editing the lists removing and adding elements, you should always pass an identifier in lists, to prevent issues.

You do so using this syntax:

```html
<script>
let goodDogs = ['Roger', 'Syd']
</script>

{#each goodDogs as goodDog (goodDog)}
  <li>{goodDog}</li>
{/each}

<!-- with the index -->
{#each goodDogs as goodDog, index (goodDog)}
  <li>{goodDog}</li>
{/each}
```

You can pass an object, too, but if your list has a unique identifier for each element, it's best to use it:

```html
<script>
let goodDogs = [
  { id: 1, name: 'Roger'},
  { id: 2, name: 'Syd'}
]
</script>

{#each goodDogs as goodDog (goodDog.id)}
  <li>{goodDog.name}</li>
{/each}

<!-- with the index -->
{#each goodDogs as goodDog, index (goodDog.id)}
  <li>{goodDog.name}</li>
{/each}
```
