Modern Web development is very much focused on components, and Svelte is no different.

What is a component? A component is an atomic part of the application, that is self-contained and optionally references other components to compose its output.

In other words, it's a compartmentalized part of the application. A form can be a component. An input element can be a component. The whole application is a component.

Svelte components contain all that's needed to render a piece of the UI. Every Svelte component is declared into a `.svelte` file, and in there you'll find the content (markup), the behavior (JavaScript) and the presentation (CSS), without having to define separate files.

Which is a sane way to define a piece of the UI, because you don't need to search for the items that affect the same element across various files.

Here's a sample component, which we'll store in a file called `Dog.svelte`:

```html
<script>
export let name;
</script>

<style>
h1 {
  color: purple;
}
</style>

<h1>The dog name is {name}!</h1>
```

Any JavaScript must be put in the `script` tag.

The CSS you have in the `style` tag is scoped to the component and does not "leak" outside. If another component has an `h1` tag, this style will not affect that. Which is very handy when reusing components you already wrote for other applications, for example.

Or when you include Open Source libraries published by other people.

For example a few weeks ago I included a date picker component built with Svelte in an application, and none of the stylings of the component leaked outside of it, and none of the CSS I wrote into the app modified the look of the date picker.

## Importing the component in other components

A component can, as said, be used by other components.

Other components can now import the `Dog` component in their code.

For example here's a `House` component, defined in a `House.svelte` file, in the same folder of `Dog.svelte`:

```html
<script>
import Dog from './Dog.svelte'
</script>
```

You can now use the Dog component like an HTML tag:

```html
<script>
import Dog from './Dog.svelte'
</script>

<Dog />
```

## Exporting specific functions from a component

As you saw above, to export the component we didn't have to do anything, because the component itself is the *default export*.

What if you want to export something other than the component markup and its associated and built-in functionality?

You must write all the functions you want to export from a special `script` tag, with the `context="module"` attribute.

Here's an example. Say you have a Button component in `Button.svelte`:

```html
<button>A button</button>
```

and you want to provide other components the ability to change the color of the button.

> A better solution for this use case is to use props, which is something we'll talk about in the next chapter. But stick with me for this example

You can provide a function, called `changeColor`.

You write and export it in this special `script` tag:

```html
<script context="module">
export function changeColor() {
  //...logic to change color..
}
</script>

<button>A button</button>
```

Note that you can have another "normal" script tag, in the component.

Now other components can import Button, which is the default export, and the `changeColor` function too:

```html
<script>
import Button, { changeColor } from './Button.svelte'
</script>
```

Now that is probably a silly example, but knowing you have this functionality at your disposal can be quite helpful.
