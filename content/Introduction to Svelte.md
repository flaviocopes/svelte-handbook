Svelte is an exciting Web framework that offers a fresh new take on how to build Web applications.

If you are already experienced in React, Vue, Angular or other frontend frameworks, you might be pleasantly surprised by Svelte.

My first impression with Svelte was that it all feels so much more plain JavaScript than working with other frameworks. Sure, you have some rules, and there are templates which are not 100% JavaScript, they look more like HTML, but most of the things that are complicated with other frameworks are very simple and lightweight with Svelte.

And my first impression has been confirmed by further usage of the framework and its ecosystem of tools.

Compared to React, Vue, Angular and other frameworks, an app built using Svelte is **compiled** beforehand, you don't have to serve the whole framework to every one of your site visitors, and as a result the fruition of the experience is smoother, consumes less bandwidth, and everything feels faster and lightweight.

At deployment, Svelte disappears and all you get is plain (and fast!) JavaScript.

## How to get started with Svelte

To use Svelte, you need to have Node.js installed because all the tooling we're going to use is based on Node. Check out my tutorial [how to install Node.js](https://flaviocopes.com/node-installation/) post if you don't have it already!

> The Svelte website provides a very cool REPL (Read-Eval-Print Loop) at <https://svelte.dev/repl>. It's handy to test small Svelte apps and experimenting with things

And make sure it's the latest version ([how to update Node.js](https://flaviocopes.com/how-to-update-node/)).

Node installs the [`npx`](https://flaviocopes.com/npx/) command, which is a handy way to run Node commands. In particular we're going to run this:

```sh
npx degit sveltejs/template firstapp
```

This will download and run the [degit command](https://github.com/Rich-Harris/degit), which in turn downloads the latest code of the Svelte project template living at [https://github.com/sveltejs/template](https://github.com/sveltejs/template), into a newly created `firstapp` folder. Make sure that [git is installed](https://git-scm.com/download) on your machine and added to the PATH variable, otherwise the degit command won't work. In case, things are still not working out for you, you can alternatively 'Clone or download' the template project and afterwards delete the hidden `.git` folder, which is basically the same what the `degit` command does (only difference is that the folder is called `template` instead of `firstapp`).

Now go into that `firstapp` folder and run `npm install` to download the additional dependencies of the template. At the time of writing, those are the dependencies of that project template:

```
"npm-run-all"
"rollup"
"rollup-plugin-commonjs"
"rollup-plugin-livereload"
"rollup-plugin-node-resolve"
"rollup-plugin-svelte"
"rollup-plugin-terser"
"svelte"
```

As you can see, it's the Svelte core, plus [Rollup](https://rollupjs.org/) (a Webpack alternative) and some of its plugins. Plus [`npm-run-all`](https://www.npmjs.com/package/npm-run-all), a CLI tool that is used to run multiple npm scripts in parallel, or sequential.

We're now ready to run our Svelte site in development mode, by running

```sh
npm run dev
```

This will start the app on localhost, on port 5000, by default:

![CLI](Introduction%20to%20Svelte/cli.png)

If you point your browser there, you'll see the "Hello world!" example:

![Browser](Introduction%20to%20Svelte/browser.png)

You're now ready to open the code in your favorite editor. The `src` folder contains all you need to tweak the app: the `main.js` file:

![Editor](Introduction%20to%20Svelte/editor.png)

This file is the entry point and in this case initializes the App component, which is defined in `App.svelte`, a single file component:

```html
<script>
export let name;
</script>

<style>
h1 {
color: purple;
}
</style>

<h1>Hello {name}!</h1>
```
