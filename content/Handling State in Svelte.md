Every component, in addition to defining the markup, the CSS and the JavaScript logic, can host its own **state**.

What is state? State is any data that's needed to make the component render what it's rendering.

For example, if a form input field has the string "test" written into it, there'll be a variable somewhere holding this value. That's the state of the input field.

The field is selected? A variable somewhere will register this fact. And so on.

State is hosted in the `script` part of a component:

```html
<script>
let count = 0
</script>
```

Now, if you come from other frameworks in the frontend space like Vue or React, you might think "how do I update this value?" - and for a good reason, as those frameworks make this operation rather unintuitive, I'd say.

One great thing about Svelte is that you don't need to do anything special to update the state of a component.

All you need is an assignment. A simple JavaScript assignment, using the `=` operator for example.

Say you have a `count` variable. You can increment that using, simply, `count = count + 1`, or `count++`:

```html
<script>
let count = 0

const incrementCount = () => {
  count++
}
</script>

{count} <button on:click={incrementCount}>+1</button>
```

This is nothing groundbreaking if you are unfamiliar with how modern Web frameworks handle state, but in React you'd have to either call `this.setState()`, or use the `useState()` hook.

Vue takes a more structured approach using classes and the `data` property.

Having used both, I find Svelte to be a much more JavaScript-like syntax.

We need to be aware of one thing, which is learned pretty quickly: we must also do an assignment when changing the value.

Svelte always want an assignment, otherwise it might not recognize that the state changed.

For simple values like strings and numbers, that's mostly a given, because all methods on String return new strings, and same for numbers - they are immutable.

But for arrays? We can't use methods that alter the array. Like `push()`, `pop()`, `shift()`, `splice()`... because there's no assignment. They change the inner data structure, but Svelte can't detect that.

Well, you *can* still use them, but after you've done your operation, you reassign the variable to itself, like this:

```js
let list = [1, 2, 3]
list.push(4)
list = list
```

Which is a bit counter-intuitive, but you'll quickly remember it.

Or you can use use the spread operator to perform operations:

```js
let list = [1, 2, 3]
list = [...list, 4]
```
