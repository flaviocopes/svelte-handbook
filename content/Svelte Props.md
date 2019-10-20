You can import a Svelte component into any other component using the syntax `import ComponentName from 'componentPath'`:

```html
<script>
import SignupForm from './SignupForm.svelte';
</script>
```

> The path is relative to the current component path. `./` means "this same folder". You'd use `../` to go back one folder, and so on.

Once you do so, you can use the newly imported component in the markup, like an HTML tag:

```html
<SignupForm />
```

In this way, you are forming a parent/child relationship between the two components: the one that imports, and the one that is imported.

Often you want to have the parent component pass data to the child component.

You can do so using **props**. Props behave similarly to attributes in plain HTML, and they are a one-way form of communication.

In this example we pass the `disabled` prop, passing the JavaScript value `true` to it:

```html
<SignupForm disabled={true}/>
```

In the SignupForm component, you need to **export** the `disabled` prop, in this way:

```html
<script>
  export let disabled
</script>
```

This is the way you express the fact that the prop is exposed to parent components.

When using the component, you can pass a variable instead of a value, to change it dynamically:


```html
<script>
import SignupForm from './SignupForm.svelte';
let disabled = true
</script>

<SignupForm disabled={disabled}/>
```

When the `disabled` variable value changes, the child component will be updated with the new prop value. Example:

```html
<script>
import SignupForm from './SignupForm.svelte';
let disabled = true
setTimeout(() => { disabled = false }, 2000)
</script>

<SignupForm disabled={disabled}/>
```