Slots are a handy way to let you define components that can be composed together.

And vice versa, depending on your point of view, slots are a handy way to configure a component you are importing.

Here's how they work.

In a component, you can define a slot using the `<slot />` (or `<slot></slot>` syntax.

Here's a `Button.svelte` component that simply prints a `<button>` HTML tag:

```html
<button><slot /></button>
```

> For React developers, this is basically the same as `<button>{props.children}</button>`

Any component importing it can define content that is going to be put into the slot, by adding it into the component opening and closing tags:

```html
<script>
import Button from './Button.svelte'
</script>

<Button>Insert this into the slot</Button>
```

You can define a default, which is used if the slot is not filled:


```html
<button>
  <slot>
    Default text for the button
  </slot>
</button>
```

You can have more than one slot in a component, and you can distinguish one from the other using named slots. The single unnamed slot will be the default one:

```html
<slot name="before" />
<button>
  <slot />
</button>
<slot name="after" />
```

Here's how you would use it:

```html
<script>
import Button from './Button.svelte'
</script>

<Button>
  Insert this into the slot
  <p slot="before">Add this before</p>
  <p slot="after">Add this after</p>
</Button>
```